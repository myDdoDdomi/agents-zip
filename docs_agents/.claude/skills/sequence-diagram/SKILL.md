---
name: sequence-diagram
description: 시퀀스 다이어그램을 작성/수정합니다. "시퀀스", "상호작용 흐름", "호출 순서도" 요청 시 사용. 사용법 /sequence-diagram [시나리오명]
---

# /sequence-diagram — 시퀀스 다이어그램

`diagram-author` 에이전트를 사용해 **시퀀스 다이어그램**을 만듭니다.

## 절차
1. `templates/시퀀스다이어그램.md` 구조를 확인한다.
2. 기능명세·API명세를 참조해 참여자와 메시지 순서를 정한다.
3. `diagram-author`에게 위임: 시나리오별 Mermaid `sequenceDiagram`, alt/opt/loop로 분기·예외, API/기능 ID 매핑.
4. 완료 후 `/doc-review` → `/drive-sync` 안내.

인자가 없으면 대상 시나리오(예: 로그인, 결제)를 먼저 묻는다.
