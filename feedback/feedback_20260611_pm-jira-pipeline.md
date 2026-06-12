# 조립 멀티팀 통합 개선 피드백 보고서 — PM·Jira 운영 파이프라인 세팅 회고

> 제출처: 에이전트 본부 / team-architect | 작성: 조립 통합 feedback-reporter
> 조립된 팀: docs, pm, design | 기간/일자: 2026-06-11 | 회고 범위: 작업 전체 (feedback/ 폴더 최초 생성 — 이전 보고서 없음)
> 한계 고지: 이 회고는 가용 신호(CLAUDE.md·MEMORY.md·에이전트 파일·산출물 흔적·사용자 제공 세션 요약) 기반 best-effort이며, 세션 내 대화·실행 로그 원본은 직접 접근 불가하여 세션 외 작업은 누락될 수 있음.
> 요약 한 줄: pm 팀 중심으로 Jira 운영 파이프라인 전체를 세팅했으나, MCP 편집 게이트 차단·이슈타입 한국어 함정·승인 게이트 형식 누락·Slack 연동 불가 등 4개 중·고위험 이슈가 에이전트 정의와 가이드 문서에 반영 대기 상태다.

---

## 1. 우선순위 요약

| # | 이슈 | 심각도 | 소속팀 | 업데이트 대상 |
|---|---|:---:|---|---|
| 1 | .mcp.json 편집 게이트 — 에이전트가 MCP 설정 파일을 생성·수정 불가(반복 2회 차단) | 🔴 | pm, design | `docs/ATLASSIAN-MCP.md`, `docs/SLACK-MCP.md`, `docs/FIGMA-MCP.md`, dev/QA 투입 체크리스트 |
| 2 | PP 이슈타입명 한국어 — `issuetype = Epic` JQL 0건, `에픽`·`스토리`·`하위 작업` 필수 | 🟠 | pm | `docs/ATLASSIAN-MCP.md`, `docs/WORKFLOW-pm.md`, `jira-analyst.md`, `sprint-planner.md`, `status-reporter.md`, `pm-lead.md`, `backlog-groomer.md`, `issue-triager.md` |
| 3 | JQL 대량 조회 토큰 초과 — searchJiraIssuesUsingJql 결과 토큰 한도 초과, 파일 저장+파싱으로 우회 | 🟠 | pm | `docs/WORKFLOW-pm.md`, `jira-analyst.md`, `pm-lead.md` |
| 4 | 승인 게이트 WRITE-GATE 표 누락으로 pm-reviewer BLOCK — 형식 미준수 시 재제출 비용 발생 | 🟠 | pm | `jira-writer.md`, `pm-lead.md`, `pm-reviewer.md`, `docs/WRITE-GATE.md` |
| 5 | Slack 연동 좌절 — 무료 플랜+비관리자 환경에서 confidential OAuth 불가, /daily-report 텍스트 폴백으로 전환 | 🟠 | pm | `docs/SLACK-MCP.md`, `slack-notifier.md`, `slack-capture.md` |
| 6 | 보안 노출 — xpp_chess .env OpenAI 키 평문, xpp_jiyu serviceAccountKey.json·.env·test_accounts.txt 노출 의심 | 🟡 | pm, docs | dev/QA 투입 체크리스트(CLAUDE.md), `docs/GETTING-STARTED.md` |
| 7 | 보고서 단정 표현 금지 규칙 — "배포 완료"·"끝남" 표현 금지 요청 반영 미완 | 🟡 | pm | `status-reporter.md`, `docs/WORKFLOW-pm.md` |
| 8 | dev/design HQ 경로 혼동 주의 — dev\design_agents(프로젝트 내)는 design HQ 아님 | 🟡 | design | `MEMORY.md` (이미 기록됨), `docs/WORKFLOW-design.md` |

---

## 2. 이슈 상세

### 이슈 1 — .mcp.json 편집 게이트 차단 (반복 2회)
**현상:** 에이전트가 `.mcp.json`(Claude Code 시작 설정 파일)을 생성·수정하려 할 때 권한 분류기가 차단한다. 이번 세션에서 ① design 팀의 figma 서버 블록 추가, ② xpp_chess·xpp_jiyu 폴더의 atlassian MCP 생성이 모두 거부돼 사용자 수동 작업으로 이관됐다.

**영향:** dev/QA 팀 투입 체크리스트 3번(`.mcp.json` 확인) 단계가 에이전트 자동화 불가 구간으로 남아, 매 프로젝트 투입 시 사용자가 수동 생성해야 한다. 문서에 이 사실이 충분히 강조돼 있지 않으면 담당자 혼동과 투입 지연이 반복된다.

**권장 업데이트:**
- `docs/ATLASSIAN-MCP.md` §1 또는 상단에 "에이전트는 `.mcp.json` 생성·수정 불가 — 사용자가 직접 파일 생성 + 브라우저 OAuth 1회" 경고 박스 추가
- `docs/SLACK-MCP.md` §3 동일 경고 추가
- `docs/FIGMA-MCP.md` 동일 경고 추가
- `CLAUDE.md` dev/QA 투입 체크리스트 3번 항목에 "(에이전트 편집 게이트로 에이전트가 생성 불가 — 사용자 직접 작성 필수)" 주석 강화

---

### 이슈 2 — PP 이슈타입명 한국어 함정
**현상:** Jira PP 프로젝트의 이슈타입 표시명이 한국어(`에픽`·`스토리`·`하위 작업`)다. 영어 JQL(`issuetype = Epic`, `issuetype = Story`)로 조회하면 0건이 반환된다.

**영향:** pm 팀 에이전트(jira-analyst·sprint-planner·status-reporter·backlog-groomer·issue-triager 등)가 에픽·스토리 집계 시 0건으로 오인식할 수 있다. 초기 조회 실패 후 재시도 비용 + 오보고 리스크.

**권장 업데이트:**
- `docs/ATLASSIAN-MCP.md`에 "PP 프로젝트 이슈타입 JQL 주의사항: `issuetype = 에픽` / `issuetype = 스토리` / `issuetype = 하위 작업` 사용" 섹션 추가
- `docs/WORKFLOW-pm.md`에 동일 내용 JQL 예시로 삽입
- `jira-analyst.md`, `sprint-planner.md`, `status-reporter.md`, `pm-lead.md`, `backlog-groomer.md`, `issue-triager.md` 각 파일의 "자가 점검" 또는 "작업 워크플로우"에 PP 이슈타입 한국어 주의 1줄 추가
- `MEMORY.md` §1(검증된 함정·우회법)에 이미 기록 완료 — 에이전트 파일 반영만 미완

---

### 이슈 3 — JQL 대량 조회 토큰 초과
**현상:** `searchJiraIssuesUsingJql` 결과가 크면 응답 토큰이 한도를 초과해 파싱 실패한다. 이번 세션에서는 결과를 파일로 저장 후 PowerShell `ConvertFrom-Json`으로 파싱하는 우회 패턴을 사용했다.

**영향:** 대형 프로젝트·스프린트 현황 조회 시 결과 손실 또는 재시도 비용. 에이전트가 우회 패턴을 모르면 실패 후 중단한다.

**권장 업데이트:**
- `docs/WORKFLOW-pm.md`에 "대량 JQL 조회 패턴" 섹션 추가: ① `maxResults`·`fields` 파라미터로 필요한 필드만 제한, ② 결과를 임시 파일로 저장 후 파싱, ③ 페이지네이션(`startAt`) 활용
- `jira-analyst.md` 작업 워크플로우 2번에 "대량이면 `maxResults`·`fields` 제한 + 페이지네이션으로 분할 조회" 강화
- `pm-lead.md` "효율 원칙"에 동일 내용 1줄 추가

---

### 이슈 4 — 승인 게이트 WRITE-GATE 표 누락으로 pm-reviewer BLOCK
**현상:** `/update_jira` 계획 제출 시 변경 계획(WRITE-GATE) 표("대상 이슈키 × 필드 × 전후값 × 영향 건수") 형식 없이 제출 → pm-reviewer가 "승인 게이트 형식 불충족" 사유로 차단. 형식을 갖춰 재제출 후 통과.

**영향:** 게이트 형식 미준수 시 Jira 반영이 1 사이클 지연된다. 에이전트 정의에 형식을 더 강하게 명시하지 않으면 반복된다.

**권장 업데이트:**
- `jira-writer.md` §승인 게이트 절차 3번 변경 계획 표 섹션에 "이 표 없이는 집행하지 않는다. pm-reviewer에 제출 전 반드시 표를 구성할 것" 강조 추가
- `pm-lead.md` §외부 발신 승인·집행(5번)에 "변경 계획 표 누락 시 pm-reviewer 차단 — 반드시 표를 먼저 완성" 경고 1줄
- `docs/WRITE-GATE.md`에 "BLOCK 사례 — 형식 누락 시 pm-reviewer가 🔴 차단, 재제출 필요" 사례 추가 (확인 필요: WRITE-GATE.md 존재 확인됨, 내용 상세는 미열람)
- `pm-reviewer.md`의 검수 판정 형식에 "변경계획표 없음 → 즉시 🔴" 명시 강화

---

### 이슈 5 — Slack 연동 좌절 및 /daily-report 텍스트 폴백 전환
**현상:** Slack MCP `authenticate` 호출 시 "dynamic client registration 미지원" 오류 반환. 원인: 무료 플랜+비관리자 환경에서는 Confidential OAuth(client_id·client_secret 필요) 앱 생성이 불가하거나 워크스페이스 앱 한도(10개) 초과. `slack-notifier`·`slack-capture` 완전 비활성화. `/daily-report`를 "복붙용 텍스트 출력" 전용으로 전환.

**영향:** 스프린트 상태 Slack 자동 게시 전면 불가. `slack-notifier`·`slack-capture` 에이전트는 Slack MCP 없이는 동작하지 않으며, 현재 환경에서 실제 사용 불가 상태. 이를 가이드·에이전트에 반영하지 않으면 담당자가 매번 같은 장벽에 부딪힌다.

**권장 업데이트:**
- `docs/SLACK-MCP.md` §3 OAuth 앱 생성 절차 앞에 "무료 플랜·비관리자 환경 주의사항" 섹션 신설: "워크스페이스 관리자 권한 없거나 앱 10개 한도 도달 시 confidential OAuth 불가 → slack-notifier/slack-capture 미동작 → 텍스트 폴백 사용"
- `slack-notifier.md` `description` 및 역할 섹션에 "Slack MCP 미연결 시 폴백: 게시할 텍스트를 복붙용으로 제공(사용자가 직접 Slack에 게시)" 명시
- `slack-capture.md` 동일 폴백 명시
- `/daily-report` 스킬 정의(CLAUDE.md)에 "자동 게시 아님, 텍스트 출력 전용(복붙)" 이미 반영됨 — 현행 유지

---

### 이슈 6 — 프로젝트 시크릿 노출 (보안)
**현상:** xpp_chess `functions/.env`에 OpenAI API 키 평문 노출(QA Sev1 — 사용자 revoke·재발급 대기). xpp_jiyu `serviceAccountKey.json`·`.env`·`test_accounts.txt` 노출 의심. (확인 필요: xpp_jiyu 파일 내용 직접 확인 미수행)

**영향:** 시크릿 노출 시 외부 악용 위험. dev/QA 팀 투입 시 시크릿 스캔이 체크리스트에 없으면 유사 사례가 반복된다.

**권장 업데이트:**
- `CLAUDE.md` dev/QA 투입 체크리스트에 "0번(선행): 프로젝트 폴더의 `.env`·`serviceAccountKey.json`·`credentials*.json` 등 시크릿 파일 존재 여부 스캔 — 노출 의심 시 사용자에게 즉시 보고 후 revoke·재발급 요청" 항목 추가
- `docs/GETTING-STARTED.md`에 "신규 프로젝트 폴더 투입 시 시크릿 스캔 선행" 강조

---

### 이슈 7 — 보고서 단정 표현 금지 규칙 미반영
**현상:** 세션 중 사용자가 "배포 완료", "끝남" 등 단정·종결 표현 금지를 요청했다. 이 규칙이 `status-reporter.md`·`docs/WORKFLOW-pm.md`에 명시돼 있지 않아 산출물 재수정 비용이 발생했다.

**영향:** 반복 교정 없이는 보고 산출물에 동일 표현이 재등장할 가능성.

**권장 업데이트:**
- `status-reporter.md` 산출물 규칙 섹션에 "단정·종결 표현 자제: Jira 완료 상태여도 '완료'·'끝남'·'배포 완료' 단정 표현을 쓰지 않고, '완료 처리됨', '상태: 완료', '반영 확인 예정' 등 사실·상태 기술 표현 사용" 추가
- `docs/WORKFLOW-pm.md` 보고서 작성 섹션에 동일 규칙 1줄 삽입
- `MEMORY.md` §3(반복 교정·선호)에 추가

---

### 이슈 8 — dev/design HQ 경로 혼동
**현상:** `dev\design_agents`(이 프로젝트 내 폴더)가 design 팀 HQ로 오인될 수 있다. 실제로는 Chesslytics 전용 구버전이며 에이전트 구성이 현재 HQ(`C:\Users\info\Desktop\team-agents\design_agents`)와 다르다.

**영향:** 팀 재조립(/team-assemble) 시 구버전 에이전트로 오조립하면 기능·정책 불일치가 발생한다.

**권장 업데이트:**
- `MEMORY.md` §2에 이미 기록됨 — 추가 조치로 `docs/WORKFLOW-design.md`에도 "HQ 레포는 `C:\Users\info\Desktop\team-agents\design_agents`" 명시 권장

---

## 3. 잘 작동한 점 (유지 권장)

1. **이슈 파이프라인 정책 전환의 신속한 수용:** "허브 일괄 반영" 모델에서 "팀 직접 반영 + 허브 보정" 모델로 세션 중 전환하고 CLAUDE.md·MEMORY.md·템플릿·README·에이전트 정의에 일관되게 반영했다. 구조 변경이 전파 누락 없이 진행된 점이 돋보인다.

2. **승인 게이트 분리 아키텍처 정착:** Jira 쓰기는 `jira-writer`만, Slack 게시는 `slack-notifier`만 담당하며, 쓰기 표면을 단일화해 감사 추적을 쉽게 만든 구조가 에이전트 정의(`pm-lead`, `jira-writer`, `pm-reviewer`)에 일관되게 구현됐다. HITL 게이트가 이번 세션에서 실제로 동작해 오반영을 방지한 사례(pm-reviewer BLOCK)가 확인됐다.

3. **1파일=1이슈 원칙과 진실원본 md 체계 확립:** `templates/이슈업데이트.md`, `update_issues/README.md`, `templates/프로젝트-파이프라인-섹션.md`가 정본화되어 xpp_chess·xpp_jiyu에 일관되게 적용됐다. md가 진실원본이고 Jira가 미러라는 개념이 모든 산출물에 일관되게 관철됐다.

4. **에픽 매핑과 기본 담당자 단순화:** "전 프로젝트 공통 최봉준", "폴더명=에픽명=담당자 이름" 규칙이 MEMORY.md에 즉시 기록되어 매 세션 재설명 비용을 제거했다.

5. **긴급(hot-add) 규칙의 현실적 설계:** `sprint: urgent` → 활성 스프린트 즉시 추가, 실패 시 md 표시 + 허브 보정이라는 폴백 체인이 실운영에서 바로 쓸 수 있는 수준으로 설계됐다.

6. **보안 노출 즉시 보고:** xpp_chess `.env` 키 노출을 투입 과정에서 발견해 사용자에게 즉시 보고하고 Sev1 이슈로 등록한 점이 적절하다.

---

## 4. 본부 액션 아이템 (체크리스트)

### pm 팀
- [ ] `docs/ATLASSIAN-MCP.md`에 PP 이슈타입 한국어 JQL 주의 섹션 추가 (이슈 2)
- [ ] `docs/ATLASSIAN-MCP.md`에 MCP 파일 편집 게이트 경고 추가 (이슈 1)
- [ ] `docs/WORKFLOW-pm.md`에 대량 JQL 조회 패턴, 이슈타입 한국어, 단정 표현 금지 규칙 반영 (이슈 2, 3, 7)
- [ ] `docs/SLACK-MCP.md` §3 앞에 무료 플랜·비관리자 폴백 케이스 섹션 추가 (이슈 5)
- [ ] `jira-analyst.md` 워크플로우에 이슈타입 한국어·대량 조회 패턴 주의 추가 (이슈 2, 3)
- [ ] `jira-writer.md` 승인 게이트 절차에 WRITE-GATE 표 필수 경고 강화 (이슈 4)
- [ ] `pm-lead.md` 워크플로우에 WRITE-GATE 표 선행 완성 경고, 대량 조회 패턴 효율 원칙 추가 (이슈 3, 4)
- [ ] `pm-reviewer.md` 검수 판정에 "변경계획표 없음 → 즉시 🔴" 명시 강화 (이슈 4)
- [ ] `status-reporter.md` 산출물 규칙에 단정 표현 금지 추가 (이슈 7)
- [ ] `slack-notifier.md` / `slack-capture.md`에 Slack MCP 미연결 폴백 명시 (이슈 5)
- [ ] `sprint-planner.md`, `status-reporter.md`, `backlog-groomer.md`, `issue-triager.md`에 이슈타입 한국어 주의 1줄 추가 (이슈 2)
- [ ] `MEMORY.md` §3(반복 교정·선호)에 단정 표현 금지 규칙 기록 (이슈 7)

### docs 팀
- [ ] `docs/GETTING-STARTED.md`에 신규 프로젝트 투입 시 시크릿 스캔 선행 강조 (이슈 6)
- [ ] `CLAUDE.md` dev/QA 투입 체크리스트 0번에 시크릿 스캔 항목 추가 (이슈 6)
- [ ] `CLAUDE.md` 투입 체크리스트 3번에 `.mcp.json` 에이전트 편집 불가 주석 강화 (이슈 1)

### design 팀
- [ ] `docs/FIGMA-MCP.md`에 MCP 파일 편집 게이트 경고 및 사용자 수동 추가 절차 명시 (이슈 1)
- [ ] `docs/WORKFLOW-design.md`에 design HQ 레포 경로 명시(dev\design_agents 혼동 방지) (이슈 8)

### 공통
- [ ] `MEMORY.md` §1(검증된 함정·우회법)에 이슈 2·3·4·5를 구체적 우회 패턴과 함께 추가 (현재 §1 비어 있음)
- [ ] Atlassian MCP 문서 URL을 SSE → Streamable HTTP(`/v1/mcp`)로 교체 확인 (MEMORY.md에 이미 기록, `docs/ATLASSIAN-MCP.md` §1 반영 여부 재확인 필요)

---

## 5. 참고 — 이번 산출물 / 근거

| 구분 | 경로 |
|---|---|
| 운영 모델 (CLAUDE.md) | `C:\Users\info\Desktop\logncoding_bong\dev\CLAUDE.md` |
| 메모리 (MEMORY.md) | `C:\Users\info\Desktop\logncoding_bong\dev\MEMORY.md` |
| 이슈 템플릿 | `C:\Users\info\Desktop\logncoding_bong\dev\templates\이슈업데이트.md` |
| 파이프라인 섹션 정본 | `C:\Users\info\Desktop\logncoding_bong\dev\templates\프로젝트-파이프라인-섹션.md` |
| update_issues README | `C:\Users\info\Desktop\logncoding_bong\dev\update_issues\README.md` |
| xpp_chess 이슈 md (10건) | `C:\Users\info\Desktop\logncoding_bong\dev\update_issues\xpp_chess\` |
| xpp_jiyu 이슈 md (1건) | `C:\Users\info\Desktop\logncoding_bong\dev\update_issues\xpp_jiyu\` |
| pm 팀 에이전트 정의 (12개) | `C:\Users\info\Desktop\logncoding_bong\dev\.claude\agents\pm\` |
| docs 팀 에이전트 정의 (17개) | `C:\Users\info\Desktop\logncoding_bong\dev\.claude\agents\docs\` |
| design 팀 에이전트 정의 (8개) | `C:\Users\info\Desktop\logncoding_bong\dev\.claude\agents\design\` |
| Atlassian MCP 가이드 | `C:\Users\info\Desktop\logncoding_bong\dev\docs\ATLASSIAN-MCP.md` |
| Slack MCP 가이드 | `C:\Users\info\Desktop\logncoding_bong\dev\docs\SLACK-MCP.md` |
| Figma MCP 가이드 | `C:\Users\info\Desktop\logncoding_bong\dev\docs\FIGMA-MCP.md` |
| WRITE-GATE 가이드 | `C:\Users\info\Desktop\logncoding_bong\dev\docs\WRITE-GATE.md` |

**근거 신호:** CLAUDE.md·MEMORY.md(세션 중 갱신본)·에이전트 md 파일·templates·update_issues·사용자 제공 세션 요약(이번 세션 함정 목록). git 이력 없음(git repo 아님).

---

## 6. 팀별 회고

### 6-1. pm 팀 — 했던 작업 요약 · 고유 이슈 · 권장 업데이트

**했던 작업 요약:**
pm 팀이 이번 세션의 핵심 주체다. Jira 운영 규칙(단일 타겟 PP·이슈 계층·[태그] 제목 10종)을 CLAUDE.md에 확정하고, 이슈 파이프라인 정책을 "허브 일괄 반영"에서 "팀 직접 반영 + 허브 보정·감사"로 전환했다. 스킬 4종(`/update_jira`, `/monday-start`, `/sunday-end`, `/daily-report`)을 CLAUDE.md에 신규 등록했다. 허브 자체 이슈 규칙(기획설계 작업의 Jira 등록, 허브 승인 게이트)을 설계해 CLAUDE.md에 반영했다. xpp_chess에 5개 이슈 md 작성 + Jira 직접 반영(PP-131~135, 137~138 — 확인 필요: 정확한 키 범위는 사용자 제공 요약 기반)을 지원했다. pm-reviewer, jira-writer, pm-lead 등 핵심 에이전트 정의가 승인 게이트 아키텍처를 일관되게 구현했다.

**pm 팀 고유 이슈:**
- PP 이슈타입 한국어 함정: JQL 에이전트들이 공통 인식해야 할 환경 특성이다. 에이전트 정의 전파 미완. (이슈 2)
- JQL 토큰 초과 우회 패턴: 대량 조회 시 발생하는 실운영 함정. 워크플로우 문서에 미기록. (이슈 3)
- pm-reviewer WRITE-GATE 표 차단: 정책이 올바르게 작동한 사례이나, 형식 준수를 에이전트 정의에 더 강하게 임베드해 재발 비용을 줄여야 한다. (이슈 4)
- Slack 연동 좌절 → 텍스트 폴백: 현재 환경에서 slack-notifier·slack-capture는 실제 동작 불가. 가이드·에이전트 정의에 폴백 케이스 미반영. (이슈 5)
- 단정 표현 금지 규칙: status-reporter·daily-report 산출물 표현 규칙이 에이전트 정의에 아직 반영되지 않음. (이슈 7)

**권장 업데이트 대상:**
`docs/ATLASSIAN-MCP.md`, `docs/WORKFLOW-pm.md`, `docs/SLACK-MCP.md`, `jira-analyst.md`, `jira-writer.md`, `pm-lead.md`, `pm-reviewer.md`, `status-reporter.md`, `slack-notifier.md`, `slack-capture.md`, `sprint-planner.md`, `backlog-groomer.md`, `issue-triager.md`, `MEMORY.md §1·§3`

---

### 6-2. docs 팀 — 했던 작업 요약 · 고유 이슈 · 권장 업데이트

**했던 작업 요약:**
docs 팀은 이번 세션에서 직접 문서 산출보다 **파이프라인 인프라 구축의 정본화**를 담당했다. `templates/이슈업데이트.md`(1파일=1이슈 프론트매터), `templates/프로젝트-파이프라인-섹션.md`(프로젝트 폴더 CLAUDE.md 복사본 정본), `update_issues/README.md`를 작성해 팀 공통 작업 표준을 확립했다. 이 산출물들이 xpp_chess·xpp_jiyu 투입의 기반이 됐다.

**docs 팀 고유 이슈:**
- 시크릿 스캔 체크리스트 미비: dev/QA 팀 투입 체크리스트에 시크릿 파일 사전 스캔 항목이 없어 투입 중 노출이 사후에 발견됐다. (이슈 6)
- `.mcp.json` 편집 불가 안내 불충분: 투입 체크리스트 3번에 에이전트 편집 불가 명시가 충분하지 않아 혼동이 반복됐다. (이슈 1, docs 관련 부분)

**권장 업데이트 대상:**
`CLAUDE.md`(투입 체크리스트 0번·3번 강화), `docs/GETTING-STARTED.md`(시크릿 스캔 선행 강조)

---

### 6-3. design 팀 — 했던 작업 요약 · 고유 이슈 · 권장 업데이트

**했던 작업 요약:**
design 팀은 이번 세션에서 직접적인 디자인 산출 작업은 없었고(확인 필요: 세션 요약에 design 팀 직접 작업 언급 없음), Figma MCP 연결 시도와 관련한 설정 작업이 있었다. `figma-operator.md`는 이미 세션 전에 정의된 상태였고, 세션 중 figma MCP 서버 블록(.mcp.json) 추가를 시도했으나 편집 게이트로 차단됐다. MEMORY.md에 "design 팀 `.mcp.json` 병합 미완" 상태로 기록됐다.

**design 팀 고유 이슈:**
- Figma MCP 설정 병합 미완: figma 서버 블록 추가가 편집 게이트로 거부돼 사용자 수동 작업이 필요한 상태. 현재 세션에서는 Figma 플러그인 MCP가 이미 연결돼 있어 당장 동작에는 지장이 없으나, 공식 `.mcp.json` 경로로 정규화되지 않은 상태다. (이슈 1)
- dev\design_agents 경로 혼동: 프로젝트 내 구버전과 HQ를 구분하지 않으면 재조립 시 오용 위험. (이슈 8)

**권장 업데이트 대상:**
`docs/FIGMA-MCP.md`(MCP 편집 게이트 경고 및 사용자 수동 추가 절차), `docs/WORKFLOW-design.md`(HQ 경로 명시), `MEMORY.md §5`(현재 기록됨 — 해소 시 삭제)
