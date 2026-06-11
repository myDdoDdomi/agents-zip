---
name: design-tokens
description: 디자인 토큰(색·타이포·스페이싱·반경·그림자)을 정의/정비합니다. "디자인 토큰", "컬러 토큰", "타이포 스케일", "토큰 정의" 요청 시 사용. 사용법 /design-tokens [도메인/테마명]
---

# /design-tokens — 디자인 토큰

`design-system-architect` 에이전트를 사용해 **디자인 토큰**을 정의합니다.

## 절차
1. `templates/디자인토큰.md` 구조를 확인한다.
2. **기존 토큰 우선**: Figma 변수·코드 토큰을 먼저 읽어 재사용/확장한다(필요 시 `figma-operator`).
   하드코딩 값이 의심되면 `figma-operator`의 **Check Designs 선행 스캔**으로 일탈 목록을 받아 토큰화 우선순위에 반영한다(MCP 호출 가능 시; 불가하면 Figma UI 수동 실행 후 결과 텍스트 수용).
   Tailwind 정합 시 프로젝트의 Tailwind 버전을 확인한다 — **(Tailwind v4 채택 시)** JS config(`tailwind.config`) 폐기, `@theme` CSS 변수(예: `--color-action-primary`) 기반 네이티브 매핑으로 진행한다.
3. `design-system-architect`에게 위임해 원시→의미→컴포넌트 계층으로 토큰을 표로 정의하고,
   색 페어의 **대비비**를 검증한다.
4. Figma와 불일치가 있으면 drift와 동기화 방향을 명시. 토큰 push(쓰기)는 `/figma-sync` + 사람 승인.

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 묻는다.
