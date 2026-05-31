# Figma 연동 가이드 (공식 MCP) + Claude Design

이 팀은 **Figma 공식(first-party) MCP 서버**로 Figma와 연결합니다. 비밀값을 레포에 넣지 않고
브라우저 OAuth로 1회 인증합니다.

## 1. 공식 Figma MCP 연결

팀 공유 설정은 레포의 `.mcp.json`에 이미 들어 있습니다:

```json
{ "mcpServers": { "figma": { "type": "http", "url": "https://mcp.figma.com/mcp" } } }
```

직접 추가하려면(개인 전역):

```bash
claude mcp add --transport http figma https://mcp.figma.com/mcp --scope user
```

- **인증**: 최초 사용 시 브라우저 OAuth로 Figma 로그인 1회. (토큰을 파일에 저장하지 않음)
- **상태 확인**: Claude Code에서 `/mcp` 로 연결·인증 상태를 확인합니다.
- **Figma 측 read-only**: 컴포넌트·변수·레이아웃·FigJam·스타일 **읽기**, 프레임→코드 생성,
  Code Connect 정합에 사용합니다. (Figma 파일을 망가뜨릴 위험이 낮은 기본 경로)

## 2. 쓰기(생성/토큰 push)가 필요할 때 — 고위험

공식 MCP는 Figma에 **쓰지 않습니다.** Figma에 노드를 생성하거나 토큰을 push해야 하면,
서드파티 쓰기형 MCP(예: `figma-console-mcp`)를 **별도로** 추가하고:

- `figma-operator` 에이전트에만 허용하고,
- 실행 전 **무엇을 어떻게 바꾸는지 보여주고 사람 승인**을 받고,
- 가능하면 **read-only 토큰·전용 계정**을 사용합니다. (보고서 리스크: 원격 쓰기 권한 주의)

비밀값(토큰)은 `.mcp.json`에 직접 넣지 말고 환경변수 `${FIGMA_TOKEN}`로 치환하며,
`.gitignore`가 `*.local.json`·`.env`·`*token*`을 제외합니다.

## 3. Claude Design (보완재 — MCP 아님)

Anthropic **Claude Design**은 프롬프트로 프로토타입·슬라이드·페이지를 만드는 **별도 제품**입니다.

- **온보딩 시 Figma·코드베이스를 읽어** 디자인 시스템(색·타이포·토큰)을 자동 구성합니다.
- 결과를 PPTX/PDF/HTML/Canva로 내보내고, 웹 프로토타입을 **Claude Code로 핸드오프**(구현 번들)합니다.
- **Figma로 직접 내보내기는 없고**, "연결하는 MCP 서버"도 아닙니다.

따라서 이 팀은 Claude Design을 **연결 대상이 아니라**, 그 **핸드오프 번들을 받아 이어가는 입구**로
취급합니다: Claude Design에서 만든 구현 번들/토큰을 이 팀의 `/design-system`·`/code-connect`로
받아 코드 정합성까지 묶습니다.

> 요약: **Figma ↔ Claude Code = 공식 MCP로 직접 연결**, **Claude Design = 인제스트+핸드오프(별도 제품)**.

## 4. 미연결(폴백) 모드

Figma MCP가 없으면 추출/생성 작업을 시도하지 말고, 마크다운 산출물(템플릿)로 작업한 뒤
연결되면 다시 동기화하도록 안내합니다.
