---
name: tailor
description: 처음부터 끝까지 기업 맞춤 이력서·포트폴리오를 만듭니다(수집→분석→작성→검수→Drive 반영). "맞춤 이력서", "이 회사 지원 준비", "전체 만들어줘" 요청 시 사용. 사용법 /tailor [GitHub ID · 포트폴리오 URL · 지원 회사/직무(JD)]
---

# /tailor — 기업 맞춤 이력서·포트폴리오 (오케스트레이션 메인)

`resume-lead` 에이전트가 전 과정을 지휘해 한 번에 맞춤 산출물을 만듭니다.

## 절차
1. **입력 확인**: GitHub ID · 포트폴리오 URL · 지원 회사/직무(JD)를 확인한다. 없는 입력은 1~2개만 질문하거나
   폴백(사용자 붙여넣기)을 안내한다.
2. **근거 수집(병렬)**: `resume-lead`가 위임한다.
   - GitHub 경력 → `github-career-extractor` (`templates/경력근거-GitHub.md`)
   - 포트폴리오 → `portfolio-analyzer` (`templates/포트폴리오분석.md`)
   - 기업·JD → `company-job-researcher` (`templates/기업직무분석.md`)
3. **맞춤 전략**: `resume-lead`가 근거 요약 + JD 정렬 기준을 만들어 "무엇을 앞세울지"를 1줄로 정한다.
4. **작성**: 이력서 → `resume-writer`(`templates/이력서.md`), 포트폴리오 → `portfolio-writer`(`templates/포트폴리오.md`).
5. **검수 게이트**: `resume-reviewer`(`docs/REVIEW-CHECKLIST.md`). 🔴가 있으면 수정 후 재검수 — 통과 전 Drive 반영 금지.
6. **Drive 반영**: `docs-formatter`(`/drive-sync`, `docs/DOC-FORMATTING.md`)로 Docs 서식→Drive 저장.
7. **통합 보고**: 무엇을·어떤 근거로 만들었는지, 남은 `(확인 필요)`와 검증 책임자(보통 사용자)를 보고한다.

> **거버넌스(절대):** 모든 경력·성과는 근거에 기반. 근거 없으면 `(확인 필요)`로 막고 사용자에게 질문한다. 추측·과장 금지. (→ CLAUDE.md §2)

인자(`$ARGUMENTS`)가 있으면 GitHub ID/포트폴리오 URL/회사·직무로 해석하고, 부족하면 무엇이 필요한지 먼저 묻는다.
