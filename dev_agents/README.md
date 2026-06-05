# Dev Agents Team 💻 — 내 프로젝트에 붙이는 "AI 개발팀"

설계·구현·코드리뷰·디버그·리팩터·마이그레이션부터 **인가된 모의해킹(레드팀) → 방어(블루팀)** 까지,
개발 한 사이클을 통째로 도와주는 **AI 팀원 16명** 묶음입니다. 내 코드/프로젝트를 열고 한국어로 시키면, 그룹장이 알맞은 팀원에게 알아서 맡깁니다.

> 📌 이 팀은 **이미 있는 코드/프로젝트 "위에서" 일하는 팀**이라, 글·자료를 새로 만드는 작성팀(문서·마케팅 등)과 **쓰는 방식이 다릅니다.** — 폴더를 여는 게 아니라, 팀원 파일을 **내 프로젝트로 복사**해서 씁니다(아래 설명).

---

## 🟢 Claude Code 처음이세요? — 설치 1분

터미널(명령을 입력하는 검은 창)을 열고 **자기 컴퓨터에 맞는 아래 한 줄을 복사 → 붙여넣기 → Enter** 하면 끝납니다.

**🍎 macOS** — `Terminal(터미널)` 앱 실행 후:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**🪟 Windows** — 시작 메뉴에서 `PowerShell` 검색해 실행 후:

```powershell
irm https://claude.ai/install.ps1 | iex
```

설치가 끝나면 같은 창에 **`claude`** 라고 입력하고 Enter 하세요. 처음 한 번만 **브라우저가 열려 로그인**(Claude Pro/Max/Team 또는 Console 계정)하면 되고, 그다음부터는 `claude`만 치면 바로 시작됩니다.

> 막히면 공식 설치 가이드: <https://code.claude.com/docs/en/quickstart> · Windows는 `Git for Windows`가 깔려 있으면 더 매끄럽습니다.

---

## 🚀 이 팀 쓰는 법 (복사해서 쓰기)

이 팀은 **내 프로젝트 폴더 안에 팀원 파일을 복사해 두면** 작동합니다. 3단계면 됩니다.

### ① Claude Code 설치
위 "설치 1분"을 끝냈으면 됩니다.

### ② 팀원 파일(`agents/`의 `*.md`)을 내 프로젝트로 복사
이 저장소의 `dev_agents/agents/` 안에 있는 `*.md` 파일들을, 내 프로젝트의 `.claude/agents/` 폴더로 복사하면 됩니다.

**🍎 macOS / Linux** (터미널):

```bash
mkdir -p 내-프로젝트/.claude/agents
cp dev_agents/agents/*.md 내-프로젝트/.claude/agents/
```

**🪟 Windows** (PowerShell):

```powershell
New-Item -ItemType Directory -Force 내-프로젝트\.claude\agents
Copy-Item dev_agents\agents\*.md 내-프로젝트\.claude\agents\
```

> 단축 명령(`/pentest`·`/remediate`·`/feedback-agents`)도 쓰려면 `dev_agents/skills/` 안 내용을 내 프로젝트의 `.claude/skills/`로 같이 복사하세요. 모의해킹 리포트 양식(`templates/침투테스트리포트.md`)도 함께 가져가면 좋습니다.
>
> macOS: `cp -r dev_agents/skills/* 내-프로젝트/.claude/skills/`
> Windows: `Copy-Item -Recurse dev_agents\skills\* 내-프로젝트\.claude\skills\`

### ③ 내 프로젝트를 열고 한국어로 시키기
복사를 마친 **내 프로젝트 폴더**에서 `claude`를 실행한 뒤, 그냥 하고 싶은 걸 한국어로 말하면 됩니다.

```
> 이 결제 함수 좀 리팩터해줘
> 방금 바꾼 코드 리뷰해줘
> 로그인할 때 가끔 튕기는 버그 원인 찾아줘
```

---

## 🤖 "에이전트"와 "스킬"이 뭐예요? (1분 개념)

어렵지 않아요. 딱 두 가지만 알면 됩니다.

- **에이전트(agents) = AI 팀원.** 각자 한 분야 전문가예요. 당신이 한국어로 시키면 **그룹장이 알맞은 팀원에게 알아서 맡깁니다.** → *팀원 이름을 외울 필요가 없어요.*
- **스킬(skills) = `/`로 시작하는 단축 명령.** 그냥 말로 시켜도 되고, 콕 집어 부를 수도 있어요. 입력창에 **`/`만 쳐도 쓸 수 있는 명령 목록**이 뜹니다.

**결론: 외울 건 없습니다. 그냥 한국어로 하고 싶은 걸 말하세요.** 아래 표는 "이 팀이 뭘 해줄 수 있는지" 참고용이에요.

---

## 👥 이 팀의 팀원(agents)

총 16명입니다. 보통은 **"그룹장(dev-lead)"에게 먼저 맡기면** 일을 쪼개서 알맞은 팀원에게 분배해 줍니다.

### 🛠 개발(만들기) 팀 — 7명
| 팀원 | 쉽게 말하면 | 무슨 일을 하나 |
|---|---|---|
| `dev-lead` | 개발 그룹장 | 계획을 세우고 일을 나눠 적합한 팀원에게 분배·통합 (먼저 부르면 좋아요) |
| `architect` | 설계자 | 어떻게 만들지 구조·기술 선택, 장단점 비교 |
| `implementer` | 구현 담당 | 실제 코드를 작성(+간단한 자체 테스트) |
| `code-reviewer` | 코드 검수자 | 바뀐 코드를 살펴 버그·품질·보안 문제를 짚어줌 (읽기 전용) |
| `debugger` | 디버거 | 버그를 재현하고 진짜 원인을 찾아 고침 |
| `refactorer` | 리팩터 담당 | 동작은 그대로 두고 코드를 더 깔끔·빠르게 정리 |
| `migration-engineer` | 마이그레이션 담당 | DB·버전·인프라 이전 작업 (위험 작업이라 사람 승인 필요) |

### 🔴 레드팀 — 인가된 모의해킹 7명
> "내 시스템에 약점이 없나" 우리 편에서 먼저 공격해 보는 보안 점검팀입니다.

| 팀원 | 쉽게 말하면 | 무슨 일을 하나 |
|---|---|---|
| `redteam-lead` | 레드팀 팀장 | **"공격해도 되는 내 자산인지" 먼저 확인**하고 모의해킹을 지휘 |
| `recon-mapper` | 정찰 담당 | 공격 가능한 표면(주소·포트·기술)을 안전하게 파악 |
| `webapp-api-pentester` | 웹/API 점검 | 웹·API의 흔한 취약점(OWASP Top 10), 로그인·세션 점검 |
| `infra-cloud-scanner` | 인프라/클라우드 점검 | 서버·클라우드 설정 실수, 보안 헤더, 인증서(TLS) 점검 |
| `code-sca-auditor` | 코드/의존성 점검 | 코드와 외부 라이브러리의 취약점·하드코딩된 비밀값 찾기 (읽기 전용) |
| `exploit-verifier` | 검증 담당 | 정말 뚫리는지 확인하고 오탐 제거 (실제 공격은 **승인 후에만**) |
| `redteam-reporter` | 보고서 작성 | 발견한 문제를 리포트로 정리해 블루팀에 넘김 |

### 🔵 블루팀(방어) + 회고 — 2명
| 팀원 | 쉽게 말하면 | 무슨 일을 하나 |
|---|---|---|
| `remediation-lead` | 방어 팀장 | 레드팀 리포트를 받아 우선순위를 정하고 수정·패치를 개발팀에 분배·재검증 |
| `feedback-reporter` | 회고 담당 | 작업을 돌아보고 본부에 전달할 개선 피드백을 정리 (주로 읽기) |

---

## ⌨️ 단축 명령(skills)

입력창에 `/`만 쳐도 목록이 뜹니다. 꼭 외울 필요는 없어요.

| 명령 | 쉽게 말하면 | 언제 쓰나 |
|---|---|---|
| `/pentest` | 모의해킹 시작 (레드팀) | 내 사이트/서비스에 보안 약점이 없는지 우리 편에서 점검할 때. (**인가·범위 확인이 먼저 뜹니다**) |
| `/remediate` | 방어 조치 시작 (블루팀) | 모의해킹에서 나온 문제를 실제로 고칠 때 |
| `/feedback-agents` | 회고 | 작업이 끝난 뒤 잘된 점·개선점을 정리해 본부에 전달할 때 |

예) `/pentest https://staging.내앱.example` → 레드팀이 **인가·범위를 먼저 확인**한 뒤 점검을 시작합니다.

---

## 💬 이렇게 말해보세요 (예시)

- "이 결제 모듈, 리팩터 계획부터 세워줘." → `dev-lead`가 계획을 짜고 팀원에게 분배
- "방금 커밋한 변경 코드리뷰 해줘." → `code-reviewer`가 버그·보안까지 점검
- "회원가입이 가끔 실패해. 원인 찾아서 고쳐줘." → `debugger`가 재현·수정
- "우리 스테이징 서버에 보안 약점 없는지 점검해줘." → `/pentest`로 레드팀이 인가 확인 후 모의해킹

---

## ⚠️ 보안 주의 (꼭 읽어주세요)

> **레드팀(`/pentest`)은 "방어 목적의 인가된 보안 테스트" 도구입니다.**
>
> - **본인 소유이거나 명시적으로 인가받은 자산에만** 사용하세요. 무단 침투·제3자 자산 공격은 **불법이며 금지**됩니다. 범위 밖·제3자·공유 클라우드 인프라 공격은 거부합니다.
> - **비파괴가 기본입니다.** 정찰·점검은 자율로 하되, **실제 공격(익스플로잇)·데이터 추출·서비스 중단(DoS)은 사람이 명시적으로 승인한 뒤에만** 실행합니다.
> - **프로덕션 안전:** 가능하면 staging/local에서 먼저, 운영 환경은 저강도로. 측면이동·범위 밖 확산은 금지합니다.
> - **발견된 비밀값·취약점 상세는 저장소(레포)에 커밋하지 마세요.** 리포트는 안전한 위치(예: `security/`·`pentest-reports/`, `.gitignore` 대상)에 남깁니다.
> - 방어 측의 PR 머지·운영 변경(의존성/스키마/인프라)도 **사람 최종 승인**을 전제로 합니다.

---
🤖 Built with [Claude Code](https://claude.com/claude-code)

---

## 🛠 고급(개발자용)

여기부터는 팀을 깊게 쓰거나 커스터마이징하려는 분을 위한 내용입니다. 처음이라면 건너뛰어도 됩니다.

### 역할 경계 (dev / QA / docs)

이 팀은 **코드를 만들고 고칩니다.** *체계적 검증*은 QA, *문서 산출물*은 docs로 넘깁니다.

| 활동 | dev_agents | QA_agents | docs_agents |
|---|---|---|---|
| 아키텍처·구현·코드리뷰·디버그·리팩터·마이그레이션 | ● | — | — |
| 단위테스트 셀프 / 코드레벨 보안 셀프 | ● | (전략·자동화·기능검증은 QA) | — |
| **인가된 침투 테스트(레드팀) → 발견 방어(블루팀)** | ● (red→report→blue) | (방어적 OWASP 보안*테스트*와 보완) | — |
| 테스트 전략·E2E·성능/접근성 *테스트* | → QA | ● | — |
| 기획서·명세서·API명세·ERD·릴리즈노트 *문서* | → docs | — | ● |

### MCP 설정 (GitHub + Context7 기본)

1. [`.mcp.json.example`](.mcp.json.example)을 대상 프로젝트로 복사 → `.mcp.json`으로 rename.
2. **기본 2개만**: **GitHub**(공식 원격, 최초 1회 브라우저 OAuth — PAT 불필요) + **Context7**(라이브러리 최신 문서).
3. **3개 이하 원칙**: 스택 MCP(Playwright/Supabase/Kubernetes/Terraform/Figma)는 **필요할 때 1개만** 추가.
4. **운영 MCP는 권한 경계**: Supabase/K8s/Terraform 등은 **`migration-engineer`에만**, **read-only·전용계정·사람 승인 게이트**.
5. **비밀값 금지**: `${ENV}` 치환, `.gitignore`가 `.mcp.json`·`.env`·`*token*`을 제외.

> 코드리뷰 등 **read-only 에이전트에 GitHub MCP를 쓰려면**, 해당 `.md`의 `tools` 줄을 제거해 전체 상속(MCP 도구 포함)으로 전환하세요(허용목록은 동적 MCP 도구명을 제외하므로).

### 협업 흐름

```
요청/이슈 → [dev-lead] 계획·분해
   ├─▶ [architect] 설계 → [implementer] 구현 → [code-reviewer] 리뷰
   ├─▶ [debugger] 버그 / [refactorer] 부채 / [migration-engineer] 마이그레이션(승인)
   ▼
[dev-lead] 통합  ──▶ 테스트는 QA_agents / 문서는 docs_agents 핸드오프
```

#### 🟣 퍼플팀 루프 (인가된 침투 테스트 → 방어)
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

### 커스터마이징
- frontmatter(`name`/`description`/`tools`/`model`)와 본문을 팀 컨벤션에 맞게 수정하세요.
- 프론트/백 동시 작업이 잦으면 `implementer`를 `frontend-dev`+`backend-dev`로 분리할 수 있습니다.
- 스택 전용 에이전트(예: `db-specialist`, `infra-engineer`)를 추가해 확장 가능합니다.

### 🧠 팀 메모리 (MEMORY.md) 사용법

이 팀은 두 층의 메모리를 쓴다.
- **CLAUDE.md** — 사람이 쓰는 안정적 규칙·라우팅(매 세션 전체 로드).
- **MEMORY.md** — 작업하며 쌓이는 학습(검증된 함정·우회법·환경 매핑·반복 교정). CLAUDE.md가 `@./MEMORY.md`로 자동 로드한다.

| 항목 | 내용 |
|---|---|
| **언제 적나** | 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · 검수가 잡아준 것 |
| **어떻게** | 해당 섹션에 **한 줄**(가능하면 날짜·근거). 규칙으로 굳으면 CLAUDE.md로 승격 |
| **확인** | 세션 중 `/memory`로 로드된 메모리 파일 확인 |
| **용량** | 200줄 이하 유지. 초과 시 `memory/<주제>.md`로 분리(필요할 때만 읽힘) |
| **개인 메모리** | 개인·머신 한정 학습은 Claude Code 자동 메모리(`~/.claude/projects/.../memory/`)가 별도 관리(레포 비커밋). 팀 공유 학습은 MEMORY.md |
| **보안** | 비밀값·취약점 상세는 MEMORY.md에 적지 않는다 — `pentest-reports/`·`security/`(.gitignore)로 |

> **(모델 B)** CLAUDE.md·MEMORY.md는 대상 프로젝트로 복사되지 않는다 — `agents/*.md`만 복사. MEMORY.md는 본부(HQ) 측 팀 기관기억이다.

### 🧩 모델·프롬프트 기준 (Claude Opus 4.8)

이 팀은 현행 **Claude Opus 4.8**(별칭 `opus`, 1M 컨텍스트) 기준으로 운영한다. 라인업·근거·전체 표는 루트 `claude-opus-4.8-운영노트.md`.

- **모델 별칭 유지.** 에이전트 `model:`은 `opus`/`sonnet`/`haiku`/`inherit` 별칭으로 둔다(모델 ID 하드코딩 금지 — Claude Code가 현행 버전으로 해석). 4.8는 천장이 높아 **effort는 `high`에서 시작**, 필요 시 상향(반사적 `max` 금지).
- **담백한 지시.** 4.8는 지시를 문자 그대로 따른다 → "CRITICAL/반드시/무조건" 같은 과장 명령은 과잉발동을 부르니 "…할 때 X" 형태로.
- **위임·도구·메모리는 트리거 명시.** 4.8는 서브에이전트 위임/검색/메모리를 보수적으로 쓴다 → `description`·도구 설명에 **"~할 때 호출"** 을 박고("여러 파일/모듈로 갈라지면 병렬"), 그룹장 루프에 "작업 전 MEMORY 확인"을 둔다.
- **코드리뷰는 "전량 보고 + 심각도".** 4.8는 "고심각만/보수적으로" 필터를 문자 그대로 따라 recall이 낮아 보인다 → **모든 발견을 신뢰도·심각도와 함께 보고, 필터는 후단계**. 장기 작업은 첫 턴에 풀스펙 + 높은 effort(`/goal`).
- **ask-rate·나레이션 보정.** 사소한 선택은 정하고 기록만(범위 변경·파괴적·배포·레드팀 익스플로잇만 사람 확인). 도구 호출 사이 불필요한 나레이션은 줄인다.
