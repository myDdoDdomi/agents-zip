---
name: code-connect
description: Figma 컴포넌트를 프로덕션 코드 컴포넌트와 매핑(Code Connect)하고 drift를 점검합니다. "Code Connect", "디자인 코드 매핑", "컴포넌트 매핑", "drift 점검" 요청 시 사용. 사용법 /code-connect [컴포넌트명]
---

# /code-connect — 디자인-코드 정합성 (Code Connect)

`code-connect-mapper` 에이전트를 사용해 **디자인-코드 매핑**을 만듭니다.

## 절차
1. `templates/CodeConnect매핑.md` 구조를 확인한다.
2. 매핑 대상 Figma 컴포넌트와 코드 컴포넌트(경로)를 확인한다. 코드가 이 레포 밖이면 경로를 받는다.
3. `code-connect-mapper`에게 위임해 컴포넌트별 prop/variant/토큰 대응을 표로 매핑하고,
   **drift(불일치)** 와 해소 방향을 적는다.
4. `figma.connect(...)` 정의가 필요하면 스니펫을 제안한다. **대상 레포 커밋은 사람 승인**(via `design-lead`).

인자(`$ARGUMENTS`)가 있으면 컴포넌트명으로 사용하고, 없으면 먼저 묻는다.
