# design-agents 🎨

> 제품 디자인의 **아이데이션 → 디자인 시스템(토큰) → Figma 추출 → 접근성·UX 검토 →
> 디자인-코드 정합(Code Connect)** 까지를 6명의 전문 에이전트로 구성한 디자인 팀입니다.
> "잘 만드는 단계"와 "나중에 틀어지지 않게 묶는 단계"를 분리해 운영합니다.

이 팀은 **모델 A(자기완결형)** — 이 폴더를 Claude Code로 **직접 열어** 슬래시 스킬과
Figma 공식 MCP를 자동 로드해 씁니다.

---

## 🚀 빠른 시작

1. **이 폴더를 Claude Code로 엽니다.**
2. **Figma를 1회 연결**합니다(브라우저 OAuth). → [Figma 연동 가이드](docs/FIGMA-MCP.md)
3. 한국어로 시킵니다. 빗금(`/`)으로 스킬을 고를 수 있습니다.
   ```
   온보딩 화면 디자인 컨셉 잡아줘
   ```
   → 컨셉 → 프로토타입 → 토큰/시스템 → 접근성 검토 → 코드 정합 순으로 진행합니다.

---

## 👥 팀 구성 (7 에이전트)

| 에이전트 | 역할 | model |
|---|---|---|
| [`design-lead`](.claude/agents/design-lead.md) | 디자인 전략·분배·종합·게이트(오케스트레이터) | opus |
| [`ideation-prototyper`](.claude/agents/ideation-prototyper.md) | 컨셉·무드·프로토타입 시나리오·화면 흐름 | sonnet |
| [`design-system-architect`](.claude/agents/design-system-architect.md) | 디자인 토큰·디자인 시스템 정의·정합 | opus |
| [`figma-operator`](.claude/agents/figma-operator.md) | Figma 추출·생성·스크린샷 (공식 MCP) | sonnet |
| [`accessibility-ux-reviewer`](.claude/agents/accessibility-ux-reviewer.md) | WCAG 접근성·UX 휴리스틱 검토(read-only) | opus |
| [`code-connect-mapper`](.claude/agents/code-connect-mapper.md) | 디자인-코드 매핑(Code Connect)·drift 점검 | sonnet |
| [`feedback-reporter`](.claude/agents/feedback-reporter.md) | 팀 작업 회고→본부 피드백 보고서(read 중심) | sonnet |

## 🧩 슬래시 스킬 (8)

| 스킬 | 담당 | 산출물 |
|---|---|---|
| `/ideate` | ideation-prototyper | 디자인 컨셉 |
| `/prototype` | ideation-prototyper | 프로토타입 시나리오 |
| `/design-tokens` | design-system-architect | 디자인 토큰 |
| `/design-system` | design-system-architect | 디자인 시스템 |
| `/figma-sync` | figma-operator | Figma 추출/생성(쓰기=사람 승인) |
| `/a11y-review` | accessibility-ux-reviewer | 접근성·UX 리포트 |
| `/code-connect` | code-connect-mapper | Code Connect 매핑 |
| `/feedback-agents` | feedback-reporter | 팀 작업 회고·피드백 보고서(→ 본부) |

---

## 🔗 Figma 연결 · Claude Design

- **Figma 공식 MCP**(`mcp.figma.com`)로 연결 — Figma 측 **read-only**(읽기·코드 생성·Code Connect).
- **쓰기**(Figma 생성/토큰 push)는 서드파티 MCP + **사람 승인 게이트**.
- **Claude Design**(Anthropic)은 *온보딩 인제스트 + Claude Code 핸드오프* 별도 제품 — 연결 MCP가
  아니라 **핸드오프 입구**로 취급. 자세히: [docs/FIGMA-MCP.md](docs/FIGMA-MCP.md).

---

## ⚠️ 보안
- Figma 토큰 등 비밀값은 레포에 올리지 않습니다. 인증은 브라우저 OAuth로 각자 1회.
- `.gitignore`가 `*.local.json`·`.env`·`*token*`을 제외합니다.
- Figma 쓰기·코드 매핑 커밋·덮어쓰기는 **사람 승인** 전제입니다.

---

## 📂 폴더 구조

```
design_agents/
├── CLAUDE.md            # 그룹장(오케스트레이터)
├── .mcp.json            # Figma 공식 MCP (비밀값 없음)
├── .claude/
│   ├── agents/          # 디자인 에이전트 7종
│   └── skills/          # 슬래시 스킬 8종
├── templates/           # 디자인 산출물 템플릿 6종
├── feedback/            # 팀 작업 회고·피드백 보고서 (→ 본부) /feedback-agents
└── docs/                # Figma MCP · 워크플로우 · 디자인 원칙
```

---
🤖 Built with [Claude Code](https://claude.com/claude-code)
