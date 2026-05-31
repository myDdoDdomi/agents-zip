---
name: ia
description: 정보구조도(IA)를 작성/수정합니다. "IA", "정보구조", "화면 계층", "사이트맵" 요청 시 사용. 사용법 /ia [서비스명]
---

# /ia — 정보구조도(IA)

`ux-architect` 에이전트를 사용해 **정보구조도(IA)** 를 만듭니다.

## 절차
1. `templates/정보구조도-IA.md` 구조를 확인한다.
2. 기능명세/요구사항을 참조해 화면·콘텐츠를 계층(Depth 1·2·3)으로 분류한다.
3. `ux-architect`에게 위임: 일관된 화면 ID 부여, Mermaid `graph` 또는 트리로 시각화.
4. 완료 후 `/doc-review` → `/drive-sync` 안내. (후속: `/menu-tree`, `/user-flow`)

인자가 없으면 대상 서비스 범위를 먼저 묻는다.
