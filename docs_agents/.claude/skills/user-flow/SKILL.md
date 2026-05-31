---
name: user-flow
description: 유저 플로우차트를 작성/수정합니다. "유저 플로우", "사용자 흐름", "플로우차트" 요청 시 사용. 사용법 /user-flow [과업명]
---

# /user-flow — 유저 플로우차트

`ux-architect` 에이전트를 사용해 **유저 플로우차트**를 만듭니다.

## 절차
1. `templates/유저플로우차트.md` 구조를 확인한다.
2. IA/메뉴트리의 화면 ID와 기능명세를 참조한다.
3. `ux-architect`에게 위임: 과업별 시작→종료 흐름을 Mermaid `flowchart`로, 분기·예외·이탈 경로 포함.
4. 완료 후 `/doc-review` → `/drive-sync` 안내. (후속: `/wireframe`)

인자가 없으면 대상 과업(예: 회원가입, 결제)을 먼저 묻는다.
