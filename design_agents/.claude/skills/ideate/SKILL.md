---
name: ideate
description: 디자인 컨셉·무드·아이데이션을 정리합니다. "컨셉", "무드", "디자인 방향", "아이디어" 요청 시 사용. 사용법 /ideate [화면/기능명]
---

# /ideate — 디자인 컨셉·아이데이션

`ideation-prototyper` 에이전트를 사용해 **디자인 컨셉**을 정리합니다.

## 절차
1. `templates/디자인컨셉.md` 구조를 확인한다.
2. (있으면) 상위 기획·IA, 기존 Figma 화면(필요 시 `figma-operator` 추출 요약)을 참조한다.
3. `ideation-prototyper`에게 위임해 문제·목표·무드·레퍼런스·방향 옵션(1~3개)을 작성한다.
   정보 부족 시 핵심부터 사용자에게 질문한다.
4. 완료 후 안내: `/prototype`(흐름·인터랙션) → `/design-tokens`·`/design-system`(정의) → `/a11y-review`(검토).

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 무엇을 디자인할지 먼저 묻는다.
