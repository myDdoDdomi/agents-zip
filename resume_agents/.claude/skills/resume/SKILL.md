---
name: resume
description: 수집된 근거와 JD 정렬 기준으로 기업 맞춤 이력서 초안을 만듭니다. "이력서 작성", "이 회사용 이력서" 요청 시 사용. 사용법 /resume [지원 회사/직무 (근거가 없으면 먼저 수집)]
---

# /resume — 기업 맞춤 이력서 작성

`resume-lead`가 근거·정렬 기준을 정리해 `resume-writer`에게 이력서 초안을 맡깁니다.

## 절차
1. `templates/이력서.md` 구조를 확인한다.
2. **근거 확인**: GitHub 경력(`/github-career`)·포트폴리오 분석(`/portfolio-analyze`)·JD 분석(`/company-research`)
   결과가 있는지 본다. 없으면 먼저 수집하거나, 핵심 근거가 비면 `(확인 필요)`로 막고 사용자에게 묻는다.
3. `resume-lead`가 근거 요약 + JD 정렬 기준을 만들어 `resume-writer`에게 위임한다.
   - JD 키워드 순서로 배치, 성과는 STAR로, 수치는 **확인값만**. 각 항목에 근거 출처 메모(검수용).
4. 완료 후 안내: `/resume-review` 검수 → `/drive-sync` Drive 반영.

> **거버넌스(절대):** 근거 없는 경력·성과·수치·스킬을 만들지 않는다. 근거 없으면 `(확인 필요)`. 기여도 부풀림 금지. (→ CLAUDE.md §2)

인자(`$ARGUMENTS`)가 있으면 지원 회사/직무로 사용하고, 없으면 어느 회사용인지 먼저 묻는다.
