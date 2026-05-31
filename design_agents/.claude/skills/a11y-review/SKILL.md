---
name: a11y-review
description: 디자인의 접근성(WCAG)·UX 휴리스틱을 검토하고 개선안을 제시합니다. "접근성", "a11y", "대비 검사", "UX 검토", "사용성 점검" 요청 시 사용. 사용법 /a11y-review [대상 화면/디자인]
---

# /a11y-review — 접근성·UX 검토

`accessibility-ux-reviewer` 에이전트를 사용해 **접근성·UX**를 검토합니다.

## 절차
1. `templates/접근성리포트.md` 구조를 확인한다.
2. 검토 대상(컨셉/토큰/프로토타입/Figma 추출)을 모은다. Figma 색·크기는 `figma-operator` 추출 요약 활용.
3. `accessibility-ux-reviewer`에게 위임해 WCAG(대비·키보드·터치타겟·대체텍스트)와 UX 휴리스틱을
   점검하고, 발견을 **표(문제·근거·심각도·개선안)** 로 정리한다.
4. 종합 판정(통과/조건부/보완)을 내리고, 보완 항목을 담당 에이전트로 라우팅한다.

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 묻는다.
