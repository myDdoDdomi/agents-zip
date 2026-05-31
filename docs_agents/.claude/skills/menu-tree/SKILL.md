---
name: menu-tree
description: 메뉴트리(내비게이션 구조)를 작성/수정합니다. "메뉴트리", "메뉴 구조", "GNB/LNB" 요청 시 사용. 사용법 /menu-tree [서비스명]
---

# /menu-tree — 메뉴트리

`ux-architect` 에이전트를 사용해 **메뉴트리**를 만듭니다.

## 절차
1. `templates/메뉴트리.md` 구조를 확인한다.
2. IA(`04_정보구조`)가 있으면 그 화면 ID 체계를 그대로 사용해 정합성을 맞춘다.
3. `ux-architect`에게 위임: GNB/LNB 계층, 항목별 화면 ID·접근 권한·노출 조건 표기.
4. 완료 후 `/doc-review` → `/drive-sync` 안내.

인자가 없으면 대상 서비스와 IA 위치를 먼저 묻는다.
