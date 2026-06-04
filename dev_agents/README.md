# Dev Agents Team 💻

소프트웨어 개발 조직을 그대로 옮긴 **Claude Code 서브에이전트(subagent) 모음**입니다.
아키텍처 설계부터 구현, 코드리뷰, 디버그, 리팩터, 마이그레이션까지 — "만드는(BUILD)" 전 과정에 더해,
**인가된 침투 테스트(레드팀) → 방어(블루팀)** 퍼플팀 루프까지 — 총 **16명의 에이전트**(개발 7 + 레드팀 7 +
블루팀 1 + 작업 회고·본부 피드백 보고용 `feedback-reporter` 1)로 구성했습니다.

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
| 단위테스트 셀프 / 코드레벨 보안 셀프 | ● | (전략·자동화·기능검증은 QA) | — |
| **인가된 침투 테스트(레드팀) → 발견 방어(블루팀)** | ● (red→report→blue) | (방어적 OWASP 보안*테스트*와 보완) | — |
| 테스트 전략·E2E·성능/접근성 *테스트* | → QA | ● | — |
| 기획서·명세서·API명세·ERD·릴리즈노트 *문서* | → docs | — | ● |

---

## 👥 팀 구성 (16 에이전트)

### 🛠 개발(BUILD) — 7
| 에이전트 | 역할 | model |
|---|---|---|
| [`dev-lead`](agents/dev-lead.md) | 계획·아키텍처·작업분해·통합·핸드오프(오케스트레이터) | opus |
| [`architect`](agents/architect.md) | 시스템/모듈 설계·기술선택·트레이드오프 | opus |
| [`implementer`](agents/implementer.md) | 풀스택 구현 + 단위테스트 셀프 | sonnet |
| [`code-reviewer`](agents/code-reviewer.md) | 코드리뷰(품질·버그·구조·코드레벨 보안), read-only | opus |
| [`debugger`](agents/debugger.md) | 버그 재현·근본원인 분석·수정 | sonnet |
| [`refactorer`](agents/refactorer.md) | 구조 개선·기술부채·성능 코드(동작 보존) | sonnet |
| [`migration-engineer`](agents/migration-engineer.md) | DB/버전/IaC 마이그레이션(운영 MCP·승인 게이트) | opus |

### 🔴 레드팀(인가된 침투 테스트) — 7
| 에이전트 | 역할 | tools(권한) | model |
|---|---|---|---|
| [`redteam-lead`](agents/redteam-lead.md) | 교전 오케스트레이터 + **인가/ROE 게이트** · 환경 탐지(prod/staging/local) | 전체 상속(MCP 의존) | opus |
| [`recon-mapper`](agents/recon-mapper.md) | 공격 표면 매핑·정찰(passive+안전 active) | Read/Grep/Glob/Bash/WebSearch/WebFetch | sonnet |
| [`webapp-api-pentester`](agents/webapp-api-pentester.md) | 웹/API·OWASP Top 10·인증/세션/JWT/OAuth | Read/Grep/Glob/Bash/WebSearch/WebFetch | opus |
| [`infra-cloud-scanner`](agents/infra-cloud-scanner.md) | 인프라/클라우드 오설정·TLS·보안헤더·IaC | Read/Grep/Glob/Bash/WebSearch/WebFetch | sonnet |
| [`code-sca-auditor`](agents/code-sca-auditor.md) | SAST·SCA·하드코딩 시크릿(레포 **read-only**) | Read/Grep/Glob/Bash | sonnet |
| [`exploit-verifier`](agents/exploit-verifier.md) | PoC 검증·오탐 제거(익스플로잇=**승인 후**) | Read/Grep/Glob/Bash/WebSearch/WebFetch | opus |
| [`redteam-reporter`](agents/redteam-reporter.md) | 침투 테스트 리포트 컴파일·블루팀 핸드오프 | Read/Grep/Glob/Bash/Write | sonnet |

### 🔵 블루팀(방어) + 회고 — 2
| 에이전트 | 역할 | model |
|---|---|---|
| [`remediation-lead`](agents/remediation-lead.md) | 리포트 기반 우선순위·수정안 설계 → dev 에이전트 분배·재검증(퍼플팀 방어 측) | opus |
| [`feedback-reporter`](agents/feedback-reporter.md) | 팀 작업 회고 → 본부 피드백 보고(read 중심) | sonnet |

> ⚖️ **레드팀은 방어 목적의 인가된 보안 테스트 도구입니다.** 본인 소유/명시 인가 자산만 대상으로 하며,
> 무단 침투는 금지됩니다. 모든 공격 에이전트에 **인가·ROE 게이트 · 비파괴 기본 · 익스플로잇 승인 게이트**가 박혀 있습니다.

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

슬래시 스킬(`/pentest`·`/remediate`·`/feedback-agents`)도 쓰려면 `skills/`를 대상 프로젝트의
`.claude/skills/`로 복사합니다. 침투 테스트 리포트 템플릿(`templates/침투테스트리포트.md`)도 함께 가져갑니다.

```bash
mkdir -p your-project/.claude/skills
cp -r dev_agents/skills/* your-project/.claude/skills/
mkdir -p your-project/templates && cp dev_agents/templates/*.md your-project/templates/
```

> 🔴🔵 **레드팀/블루팀:** `redteam-*`·`recon-mapper`·`webapp-api-pentester`·`infra-cloud-scanner`·
> `code-sca-auditor`·`exploit-verifier`·`remediation-lead` 도 `agents/*.md` 복사 시 함께 따라갑니다.
> 침투 테스트 리포트는 **레포에 커밋하지 말고** 안전 위치(예: `security/`·`pentest-reports/`, `.gitignore` 대상)에 산출하세요.

### 2) 개인 전역(global)
모든 프로젝트에서 쓰려면 `~/.claude/agents/`(Windows: `%USERPROFILE%\.claude\agents\`)로 복사.

### 3) 호출
```
> dev-lead 로 결제 모듈 리팩터 계획을 세워줘
> code-reviewer 로 이번 변경(diff)을 리뷰해줘
> /pentest https://staging.myapp.example  (인가·ROE 확인 후 레드팀 교전 시작)
> /remediate  (레드팀 리포트 기반 방어 시작 — 블루팀)
```
> 💡 보통은 **`dev-lead`에게 먼저 맡기면** 계획·분해 후 적합한 에이전트에 분배합니다.
> 🔴 침투 테스트는 **`/pentest`(또는 `redteam-lead`)**로 시작하면 **인가·ROE를 먼저 확인**한 뒤 교전합니다.

> 🔁 **작업이 끝나면 회고:** `/feedback-agents`(또는 "feedback-reporter로 이번 개발 작업 회고해줘")로
> 이 팀의 작업을 돌아보고, 본부(team-architect)에 전달할 `feedback.md`를 **대상 프로젝트의 `feedback/`**에
> 산출합니다. best-effort 회고(git·세션·산출물 흔적 기반)이며, 본부가 dev 팀 정의·문서를 바로 개선하도록 업데이트 대상을 적습니다.

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

### 🟣 퍼플팀 루프 (인가된 침투 테스트 → 방어)
```
/pentest → [redteam-lead] 인가·ROE 게이트 → 환경 탐지(prod/staging/local)
   ├─▶ [recon-mapper] 표면 매핑   ├─▶ [webapp-api-pentester] 웹/API/OWASP
   ├─▶ [infra-cloud-scanner] 인프라/클라우드   ├─▶ [code-sca-auditor] SAST/SCA/시크릿(read-only)
   ├─▶ [exploit-verifier] PoC 검증·오탐 제거 (익스플로잇=승인 후)
   ▼
[redteam-reporter] 침투 테스트 리포트(안전 위치·레포 커밋 금지)
   ▼  /remediate
[remediation-lead] 우선순위·수정안 → implementer/debugger/refactorer/migration-engineer → code-reviewer
   ▼
[exploit-verifier/redteam-lead] 재검증 → 루프 닫기  (머지·운영 변경은 사람 승인)
```
> ⚖️ **인가된 자산만.** 범위 밖·제3자·공유 클라우드 인프라 공격은 거부합니다. 익스플로잇·데이터 추출·DoS는
> 명시 승인 전제(비파괴 기본). 발견 시크릿·취약점 상세는 레포에 커밋하지 않습니다.

---

## 🛠 커스터마이징
- frontmatter(`name`/`description`/`tools`/`model`)와 본문을 팀 컨벤션에 맞게 수정하세요.
- 프론트/백 동시 작업이 잦으면 `implementer`를 `frontend-dev`+`backend-dev`로 분리할 수 있습니다.
- 스택 전용 에이전트(예: `db-specialist`, `infra-engineer`)를 추가해 확장 가능합니다.

---
🤖 Built with [Claude Code](https://claude.com/claude-code)
