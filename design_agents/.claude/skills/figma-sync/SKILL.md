---
name: figma-sync
description: Figma에서 추출/생성/스크린샷/버전 조회를 수행합니다(공식 MCP, 읽기 기본·쓰기는 사람 승인). "Figma 추출", "Figma 읽어줘", "스크린샷", "Figma에 만들어줘" 요청 시 사용. 사용법 /figma-sync [Figma 링크/대상]
---

# /figma-sync — Figma 연동 (공식 MCP)

`figma-operator` 에이전트를 사용해 Figma와 연동합니다.

## 사전 확인
1. Figma 공식 MCP(`mcp.figma.com`) 연결 여부를 확인한다(도구 검색).
   - **연결 안 됨**: 작업을 시도하지 말고 `docs/FIGMA-MCP.md`로 연결(브라우저 OAuth 1회)을 안내하고
     마크다운 폴백으로 전환한다.
2. 대상 Figma 파일/프레임/노드 링크를 확인한다.

## 절차
1. **읽기(기본·안전)**: `figma-operator`에게 위임해 컴포넌트·변수·레이아웃·스타일을 **최소 범위만**
   추출하고 핵심 요약+표로 전달한다(중복 추출 금지).
2. **쓰기(고위험)**: Figma에 생성/토큰 push가 필요하면 무엇을 어떻게 바꾸는지 먼저 보여주고
   **사람 승인**을 받은 뒤에만 실행한다(서드파티 쓰기형 MCP 필요).
3. 결과를 요청한 작업(`/design-tokens`, `/code-connect` 등)으로 넘긴다.

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 Figma 링크를 먼저 묻는다.
