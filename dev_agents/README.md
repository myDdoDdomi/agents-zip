# Dev Agents Team 💻

소프트웨어 개발 조직을 그대로 옮긴 **Claude Code 서브에이전트(subagent) 모음**입니다.
아키텍처 설계부터 구현, 코드리뷰, 디버그, 리팩터, 마이그레이션까지 — "만드는(BUILD)" 전 과정을
**7명의 전문 개발 에이전트**로 구성했습니다.

루트 [`CLAUDE.md`](CLAUDE.md)의 **개발 그룹장**과 [`dev-lead`](agents/dev-lead.md)가
**오케스트레이터**로서 작업을 분배·통합하고, 테스트는 **QA_agents**, 문서는 **docs_agents**로
핸드오프합니다.

> 🧭 **2026 정렬**: 공식 스택(서브에이전트+Skills+MCP), 모델 티어링(Opus/Sonnet), 최소권한·권한경계,
> GitHub·Context7 MCP, Human-in-the-loop를 설계에 반영했습니다.

---

## 🚧 역할 경계 (dev / QA / docs)

이 팀은 **코드를 만들고 고칩니다.** *체계적 검증*은 QA, *문서 산출물*은 docs로 넘깁니다.

| 활동 | dev_agents | QA_agents | docs_agents |
|---|---|---|---|
| 아키텍처·구현·코드리뷰·디버그·리팩터·마이그레이션 | ● | — | — |
| 단위테스트 셀프 / 코드레벨 보안 셀프 | ● | (전략·자동화·OWASP·침투 보안은 QA) | — |
| 테스트 전략·E2E·성능/접근성/보안 *테스트* | → QA | ● | — |
| 기획서·명세서·API명세·ERD·릴리즈노트 *문서* | → docs | — | ● |

---

## 👥 팀 구성 (7 에이전트)

| 에이전트 | 역할 | model |
|---|---|---|
| [`dev-lead`](agents/dev-lead.md) | 계획·아키텍처·작업분해·통합·핸드오프(오케스트레이터) | opus |
| [`architect`](agents/architect.md) | 시스템/모듈 설계·기술선택·트레이드오프 | opus |
| [`implementer`](agents/implementer.md) | 풀스택 구현 + 단위테스트 셀프 | sonnet |
| [`code-reviewer`](agents/code-reviewer.md) | 코드리뷰(품질·버그·구조·코드레벨 보안), read-only | opus |
| [`debugger`](agents/debugger.md) | 버그 재현·근본원인 분석·수정 | sonnet |
| [`refactorer`](agents/refactorer.md) | 구조 개선·기술부채·성능 코드(동작 보존) | sonnet |
| [`migration-engineer`](agents/migration-engineer.md) | DB/버전/IaC 마이그레이션(운영 MCP·승인 게이트) | opus |

---

## 🚀 설치 / 사용 방법

`agents/*.md`는 [Claude Code 서브에이전트](https://code.claude.com/docs/en/sub-agents) 형식입니다.
루트 [`CLAUDE.md`](CLAUDE.md)는 이 리포에서 작업할 때 자동 로드되는 그룹장 지침입니다.

### 1) 프로젝트에 추가 (팀 공유 권장)
```bash
git clone <this-repo-url>
mkdir -p your-project/.claude/agents
cp dev_agents/agents/*.md your-project/.claude/agents/
```

### 2) 개인 전역(global)
모든 프로젝트에서 쓰려면 `~/.claude/agents/`(Windows: `%USERPROFILE%\.claude\agents\`)로 복사.

### 3) 호출
```
> dev-lead 로 결제 모듈 리팩터 계획을 세워줘
> code-reviewer 로 이번 변경(diff)을 리뷰해줘
```
> 💡 보통은 **`dev-lead`에게 먼저 맡기면** 계획·분해 후 적합한 에이전트에 분배합니다.

---

## 🔌 MCP 설정 (GitHub + Context7 기본)

1. [`.mcp.json.example`](.mcp.json.example)을 대상 프로젝트로 복사 → `.mcp.json`으로 rename.
2. **기본 2개만**: **GitHub**(공식 원격, 최초 1회 브라우저 OAuth — PAT 불필요) + **Context7**(라이브러리 최신 문서).
3. **3개 이하 원칙**: 스택 MCP(Playwright/Supabase/Kubernetes/Terraform/Figma)는 **필요할 때 1개만** 추가.
4. **운영 MCP는 권한 경계**: Supabase/K8s/Terraform 등은 **`migration-engineer`에만**, **read-only·전용계정·사람 승인 게이트**.
5. **비밀값 금지**: `${ENV}` 치환, `.gitignore`가 `.mcp.json`·`.env`·`*token*`을 제외.

> 코드리뷰 등 **read-only 에이전트에 GitHub MCP를 쓰려면**, 해당 `.md`의 `tools` 줄을 제거해
> 전체 상속(MCP 도구 포함)으로 전환하세요(허용목록은 동적 MCP 도구명을 제외하므로).

---

## 🔄 협업 흐름

```
요청/이슈 → [dev-lead] 계획·분해
   ├─▶ [architect] 설계 → [implementer] 구현 → [code-reviewer] 리뷰
   ├─▶ [debugger] 버그 / [refactorer] 부채 / [migration-engineer] 마이그레이션(승인)
   ▼
[dev-lead] 통합  ──▶ 테스트는 QA_agents / 문서는 docs_agents 핸드오프
```

---

## 🛠 커스터마이징
- frontmatter(`name`/`description`/`tools`/`model`)와 본문을 팀 컨벤션에 맞게 수정하세요.
- 프론트/백 동시 작업이 잦으면 `implementer`를 `frontend-dev`+`backend-dev`로 분리할 수 있습니다.
- 스택 전용 에이전트(예: `db-specialist`, `infra-engineer`)를 추가해 확장 가능합니다.

---
🤖 Built with [Claude Code](https://claude.com/claude-code)
