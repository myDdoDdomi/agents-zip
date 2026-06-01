---
name: github-career
description: GitHub 레포·기여·스택을 '경력 근거'로 추출·정리합니다. "GitHub 경력", "깃허브 분석", "레포 정리" 요청 시 사용. 사용법 /github-career [GitHub 사용자/조직 · (선택) 대상 레포]
---

# /github-career — GitHub 경력 근거 추출

`github-career-extractor` 에이전트가 GitHub 활동을 읽어 **경력 근거**(레포·기여·스택)를 정리합니다.

## 절차
1. `templates/경력근거-GitHub.md` 구조를 확인한다.
2. 대상 GitHub 사용자/조직과 범위(전체/특정 레포)를 확인한다. 모호하면 1~2개만 질문.
3. `github-career-extractor`에게 위임 → **GitHub MCP(읽기 전용)** 로 레포·기여·언어를 읽어 근거를 정리한다.
   - 모든 항목에 출처(레포 URL). 기여도(주도/공동/포크)는 사실대로. 불확실하면 `(확인 필요)`.
4. 완료 후 안내: `/company-research`(JD 정렬 기준) → `/resume`/`/portfolio` 작성.

> **거버넌스:** 실제 레포·기여만 근거로. fork·연습 레포를 "주도 개발"로 부풀리지 않는다. (→ CLAUDE.md §2, §9)
> MCP 미연결이면 사용자가 레포 목록·README·기여를 붙여넣어 폴백 처리한다.

인자(`$ARGUMENTS`)가 있으면 GitHub 사용자/조직·레포로 사용하고, 없으면 누구의 GitHub인지 먼저 묻는다.
