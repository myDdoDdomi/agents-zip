# pm-agents 표준 워크플로우

이 팀의 작업 루프와 각 단계의 담당·산출물·게이트를 정의합니다. (요약은 CLAUDE.md §4)

## PM 루프 (5단계)

```
① 입력(Input)    → 대상 프로젝트/보드 · 요청(현황/정리/계획/보고/반영)
② 현황 수집(Read) → JQL 검색·조회로 현재 상태 파악 (jira-analyst) → 근거 JQL + 조회 시점
③ 가공(Plan)     → 백로그 정리 / 스프린트 계획 / 트리아지 / 보고서 → 평문 .md 파일
④ 검수(Review)   → 정합·실행가능성·승인 게이트 준수 (pm-reviewer) → 검수 판정 블록(✅/🔴)
⑤ 반영(Apply)    → 검수 ✅ + 사용자 승인 시에만 Jira 쓰기 (jira-writer / /jira-apply)
```

## 단계별 상세

### ② 현황 수집 (`jira-analyst`)
- `searchJiraIssuesUsingJql`로 대상 이슈를 조회. 상태/담당/우선순위/타입 분포 + 정체·블로커.
- 모든 수치에 **사용 JQL + 조회 시점**. 산출: `templates/현황분석.md`.
- `pm-lead`가 결과를 **핵심 요약**으로 가공해 다음 단계에 전달(각자 재조회 금지).
- **JQL 주의:** 이슈타입·상태 표시명이 현지어인 사이트면 영어 타입명 JQL이 0건이다 — 표시명 확인 후
  재조회(`docs/ATLASSIAN-MCP.md` §3-1). **대량 조회**는 `fields`·`maxResults` 제한 + `startAt`
  페이지네이션으로 분할, 토큰 한도 초과 시 파일 저장 후 파싱(§3-2).

### ③ 가공 (요청에 따라)
| 요청 | 담당 | 템플릿 |
|---|---|---|
| 백로그 정리 | `backlog-groomer` | `백로그정리.md` |
| 스프린트 계획 | `sprint-planner` | `스프린트계획.md` |
| 트리아지 | `issue-triager` | `트리아지.md` |
| 상태 보고 | `status-reporter` | `상태보고서.md` |
- 독립 작업은 병렬 위임. 산출물은 평문 `.md`(단일 원본).
- **보고서 표현 규칙(status-reporter):** "배포 완료"·"끝남" 같은 단정·종결 표현 대신 "완료 처리됨"·
  "상태: 완료(Done)"·"반영 확인 예정"처럼 **사실·상태 기술**로 적는다(Jira 상태 ≠ 실배포 검증).

### ④ 검수 (`pm-reviewer`)
- `docs/REVIEW-CHECKLIST.md` 기준. 정합·실행가능성·**승인 게이트 준수**·완성도.
- 산출: 검수 판정 블록(✅/🔴 + 발견 목록). 🔴 있으면 불통과 → 수정 후 재검수.

### ⑤ 반영 (`jira-writer` / `/jira-apply`)
- **승인 게이트** 통과 후에만(→ `docs/WRITE-GATE.md`). 변경 계획 표 → 사용자 승인 → 집행 → 결과 보고.

## git↔Jira 추적성 (`/trace`) — 그리고 PR머지 → Jira 갱신 경로

`git-traceability`가 GitHub(커밋·PR·브랜치)와 Jira(Epic/Story/Task)를 이슈키(`PROJ-123`)로 대조한다.
**추적 분석은 read-only**, **갱신은 초안까지만** — 실제 Jira 쓰기는 기존 `/jira-apply` 게이트로 수렴한다.

```
/trace
 ② 수집(Read)   → GitHub 읽기(또는 로컬 git log) + Jira Epic/Story/Task (git-traceability, read-only)
 ③ 분석(.md)    → 커버리지 매트릭스 · 고아 커밋/PR · dev 활동 없는 이슈 · 이슈↔PR 매핑
                  └ (요청 시) PR머지/브랜치 근거 → "Jira 갱신 후보 *초안*" 표 (반영 아님)
 ④ 검수(Review) → 정합·근거·게이트 준수 (pm-reviewer)
 ⑤ 반영(Apply)  → /jira-apply (jira-writer) ─ git 출처 갱신도 동일 승인 게이트 ─ 코멘트/전이/워크로그
```

- **read-only:** `git-traceability`는 GitHub 쓰기 도구가 없다(PR/브랜치 생성 안 함). 갱신은 초안 표까지만.
- **동일 게이트:** git 출처(PR 머지 근거) 갱신도 일반 Jira 쓰기와 **같은 승인 게이트**(`docs/WRITE-GATE.md`).
- **자동 전이 금지:** PR 머지가 곧 이슈 완료는 아니다 — 전이는 *후보*로만, 사람 승인에 맡긴다.
- 이슈키 규칙·GitHub MCP 연결·읽기 도구 범위는 `docs/GITHUB-MCP.md`.

## 한 방에 끝내기 (오케스트레이션 스킬)
- `/sprint-plan`: 현황 → 계획 → 검수 (→ 승인 시 반영).
- `/triage`: 분류 → 제안 (→ 승인 시 반영).
- `/trace`: git↔Jira 추적성 분석 → 검수 (→ 갱신 초안은 `/jira-apply` 게이트로 반영).
- 읽기만: `/jira-status`, `/backlog-groom`, `/status-report`, `/trace`. 쓰기: `/jira-apply`(승인 게이트).

## 폴백 (MCP 미연결)
- Atlassian 도구가 없으면 사용자가 붙여넣은 이슈 목록/JQL 결과로 분석·계획을 산출하고,
  쓰기 반영은 "수동으로 적용할 변경 목록"으로 제공한다.
