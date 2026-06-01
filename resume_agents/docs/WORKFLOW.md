# 작업 워크플로우 (맞춤화 루프)

이 팀의 모든 작업은 아래 루프를 따릅니다. 그룹장(`resume-lead`)이 오케스트레이션합니다.

```
① 입력(Input)      → GitHub ID · 포트폴리오 URL · 지원 기업/직무(JD)
② 근거 수집(Gather) → [GitHub 경력 / 포트폴리오 / 기업·JD] 병렬 분석 (근거+출처)
③ 작성(Write)      → 기업 맞춤 이력서 / 포트폴리오 초안 (JD 키워드 정렬·STAR)
④ 검수(Review)     → 사실 정합·기업 적합도·과장/금칙 게이트 (resume-reviewer)
⑤ 반영(Sync)       → Google Docs 서식 → Drive 저장·정리 (/drive-sync)
```

## 단계별 안내

### ① 입력
- 필요한 3가지: **GitHub ID**, **포트폴리오 URL**, **지원 기업/직무(JD)**.
- 없는 입력이 있으면 그룹장이 1~2개만 질문하거나 폴백(사용자 붙여넣기)을 안내합니다.

### ② 근거 수집 (병렬)
세 작업은 독립이므로 동시에 진행합니다.

| 수집 | 스킬 | 에이전트 | 산출 |
|---|---|---|---|
| GitHub 경력 | `/github-career` | `github-career-extractor` | `templates/경력근거-GitHub.md` |
| 포트폴리오 | `/portfolio-analyze` | `portfolio-analyzer` | `templates/포트폴리오분석.md` |
| 기업·JD | `/company-research` | `company-job-researcher` | `templates/기업직무분석.md` |

- 모든 항목에 **출처**가 붙어야 합니다. 근거 없는 사실은 `(확인 필요)`.
- 그룹장은 세 결과를 한 번 읽어 **핵심 근거 요약**을 만들어 작성 에이전트에 넘깁니다(각자 재크롤 금지).

### ③ 작성
- JD 분석이 "정렬 기준"입니다. **무엇을 앞세울지**를 정한 뒤 작성합니다.
- 이력서 `/resume`(`resume-writer`), 포트폴리오 `/portfolio`(`portfolio-writer`).
- 각 항목에 근거 출처 메모(검수용)를 답니다. 성과는 STAR, 수치는 확인값만.

### ④ 검수 (게이트)
- `/resume-review`(`resume-reviewer`)가 **사실 정합·기업 적합도·과장/금칙·완성도**를 점검.
- 🔴(필수)가 하나라도 있으면 **불통과** → 작성자가 수정 후 재검수. 통과 전 Drive 반영 금지.

### ⑤ 반영
- `/drive-sync`(`docs-formatter`)가 Google Docs 네이티브 서식을 적용해 Drive에 저장.
- 기존 문서 덮어쓰기 전 현재 내용 확인(Human-in-the-loop).

## 한 방에 끝내기 — `/tailor`
`resume-lead`가 ①~⑤를 한 번에 오케스트레이션합니다. 복잡하거나 처음부터 끝까지 만들 때 사용하세요.

## 효율 모드 (그룹장이 선택)
- **빠르게**: 단건 수정·단일 근거 추출·간단 검수.
- **표준(기본)**: 한 종류 작성·검수·기업 분석.
- **심화**: `/tailor` 전체 맞춤, 여러 기업 동시 지원, 교차검증이 값을 할 때(그룹장이 제안→승인).

> §최우선 거버넌스(CLAUDE.md §2)는 효율보다 항상 우선합니다 — 추측 경력·허위 스펙 금지.
