---
name: design-system
description: 디자인 시스템(컴포넌트 인벤토리·상태·변형·사용 원칙)을 정의/정비합니다. "디자인 시스템", "컴포넌트 인벤토리", "컴포넌트 원칙" 요청 시 사용. 사용법 /design-system [서비스/시스템명]
---

# /design-system — 디자인 시스템

`design-system-architect` 에이전트를 사용해 **디자인 시스템**을 정리합니다.

## 절차
1. `templates/디자인시스템.md` 구조를 확인한다.
2. **기존 우선**: Figma/코드의 기존 컴포넌트를 먼저 읽어 인벤토리화한다(중복 신설 금지).
3. `design-system-architect`에게 위임해 컴포넌트별 상태(state)·변형(variant), 사용/금지 원칙,
   토큰 매핑을 정리한다.
4. 코드 매핑이 필요하면 `/code-connect`로, 접근성 점검은 `/a11y-review`로 넘긴다.

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 묻는다.
