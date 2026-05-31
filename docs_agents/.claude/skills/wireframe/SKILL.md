---
name: wireframe
description: 와이어프레임(저충실도 화면 레이아웃)을 작성/수정합니다. "와이어프레임", "화면 레이아웃", "목업 구조" 요청 시 사용. 사용법 /wireframe [화면명]
---

# /wireframe — 와이어프레임

`ux-architect` 에이전트를 사용해 **텍스트 와이어프레임**을 만듭니다.

## 절차
1. `templates/와이어프레임.md` 구조를 확인한다.
2. 화면 ID(IA/메뉴트리)와 유저플로우를 참조한다.
3. `ux-architect`에게 위임: 화면별 ASCII/박스 레이아웃, 요소 번호 + 디스크립션 표(역할/동작).
4. 완료 후 `/doc-review` → `/drive-sync` 안내. (후속: `/screen-spec`)

인자가 없으면 대상 화면을 먼저 묻는다.
