---
name: functional-spec
description: 기능명세서를 작성/수정합니다. "기능명세", "기능 사양", "동작 규칙 정의" 요청 시 사용. 사용법 /functional-spec [기능명]
---

# /functional-spec — 기능명세서

`spec-writer` 에이전트를 사용해 **기능명세서**를 만듭니다.

## 절차
1. `templates/기능명세서.md` 구조를 확인한다.
2. 요구사항 정의서(`02_요구사항`)를 참조해 FR을 구현 가능한 명세로 전개한다.
3. `spec-writer`에게 위임: 기능 ID·입력·처리규칙·출력·권한·예외·유효성, 상태 전이, 비즈니스 규칙표.
4. 완료 후 `/doc-review` → `/drive-sync` 안내.

인자가 없으면 대상 기능과 요구사항 문서 위치를 먼저 묻는다.
