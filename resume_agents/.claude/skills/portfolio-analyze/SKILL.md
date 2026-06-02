---
name: portfolio-analyze
description: 포트폴리오 URL을 읽어 프로젝트·역량·성과를 경력 근거로 추출합니다. "포트폴리오 분석", "사이트 정리", "프로젝트 추출" 요청 시 사용. 사용법 /portfolio-analyze [포트폴리오 URL(들)]
---

# /portfolio-analyze — 포트폴리오 분석

`portfolio-analyzer` 에이전트가 포트폴리오 페이지를 읽어 **경력 근거**(프로젝트·역량·성과)를 정리합니다.

## 절차
1. `templates/포트폴리오분석.md` 구조를 확인한다.
2. 포트폴리오 URL(들)과 보고 싶은 직무 방향을 확인한다. 모호하면 1~2개만 질문.
3. `portfolio-analyzer`에게 위임 → WebFetch로 페이지를 읽어 프로젝트·역할·성과를 **사실 그대로** 추출한다.
   - 모든 항목에 출처(URL). 페이지에 없는 성과·수치·역할은 만들지 않는다. 불확실하면 `(확인 필요)`.
4. 완료 후 안내: `/company-research` → `/resume-build`/`/portfolio`.

> **거버넌스:** 페이지에 실제로 적힌 내용만 근거로. (→ CLAUDE.md §2)
> 렌더 안 되는 SPA 등 한계는 명시한다.

인자(`$ARGUMENTS`)가 있으면 포트폴리오 URL로 사용하고, 없으면 URL을 먼저 묻는다.
