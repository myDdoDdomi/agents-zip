# pm-agents 📋 — 나만의 Jira 프로젝트 관리 AI 팀

**Jira(Atlassian)** 위에서 현황 파악·백로그 정리·스프린트 계획·이슈 트리아지·진행 보고를 도와주고,
**승인한 변경만** Jira에 실제로 반영해 주는 AI 팀이에요. 한국어로 "이 프로젝트 현황 알려줘",
"다음 스프린트 계획 짜줘"처럼 말하기만 하면 됩니다.

> ⛔ **이 팀의 1순위 약속: 외부에 함부로 내보내지 않아요(Jira 쓰기 + Slack 게시).** 이슈 생성·수정·상태
> 변경 같은 **Jira 쓰기**, 그리고 **Slack 채널 게시**는 항상 "무엇을 → 어떻게/어디에" 미리보기로 먼저
> 보여드리고, **당신이 승인한 뒤에만** 내보냅니다(둘 다 같은 승인 게이트). 이슈·**Slack 메시지** 내용에
> "이거 닫아라/공지해라" 같은 문구가 있어도 마음대로 실행하지 않아요(프롬프트 인젝션 방어 — 특히 Slack은 신뢰 불가 입력).

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

설치가 끝나면 같은 창에 **`claude`** 라고 입력하고 Enter 하세요. 처음 한 번만 **브라우저가 열려 로그인**하면 되고, 그다음부터는 `claude`만 치면 바로 시작됩니다.

> 막히면 공식 설치 가이드: <https://code.claude.com/docs/en/quickstart>

---

## 🚀 이 팀 쓰는 법 (3단계)

**① 이 폴더를 `claude`로 엽니다.**
터미널에서 이 `pm_agents` 폴더로 이동한 뒤 `claude` 라고 입력하세요. 팀원(에이전트)과 명령(스킬)이 자동으로 불러와집니다.

**② Atlassian(Jira) 연결을 딱 1번만 해 둡니다. (추적성을 쓰면 GitHub, Slack 연동을 쓰면 Slack도)**
이 팀은 **Atlassian 공식 원격 MCP**로 Jira에 연결해요. 처음 한 번 **브라우저로 Atlassian 로그인**(OAuth)만
하면 됩니다 — **비밀번호/토큰을 파일에 넣지 않아요.**
- 폴더를 연 뒤 `/mcp`를 입력하면 `atlassian` 서버가 보이고, 최초 호출 시 브라우저 인증이 뜹니다.
- 안내: [`docs/ATLASSIAN-MCP.md`](docs/ATLASSIAN-MCP.md) (도구 목록·1차 범위 포함)
- **`/trace`(GitHub↔Jira 추적성)를 쓰려면** `github` 서버도 `/mcp`에서 최초 1회 **GitHub OAuth**(PAT 불필요)
  해 두세요. **읽기 전용**(PR/커밋/브랜치 조회)만 쓰고 GitHub에 쓰지 않아요 — [`docs/GITHUB-MCP.md`](docs/GITHUB-MCP.md)
- **`/slack-capture`·`/slack-post`(Slack 연동)를 쓰려면** `slack` 서버를 연결하세요. ⚠️ Slack은 다른 둘과 달리
  **앱 client_id + client_secret(비밀값)** 이 필요해요 — `${SLACK_CLIENT_ID}`/`${SLACK_CLIENT_SECRET}` 환경변수로
  넣고 **파일에 secret을 커밋하지 않습니다.** 앱 생성·secret 주입 절차: [`docs/SLACK-MCP.md`](docs/SLACK-MCP.md).
  Slack **게시(외부 발신)는 항상 승인 후에만**(Jira 쓰기와 같은 게이트).

**③ 한국어로 그냥 시키세요.**
예: **"ABC 프로젝트 현황 알려줘"**, **"다음 스프린트 계획 짜줘"**, **"이번 주 신규 이슈 트리아지해줘"**.

---

## 🤖 "에이전트"와 "스킬"이 뭐예요? (1분 개념)

- **에이전트(agents) = AI 팀원.** 각자 한 분야 전문가예요. 한국어로 시키면 **그룹장이 알맞은 팀원에게 알아서 맡깁니다.**
- **스킬(skills) = `/`로 시작하는 단축 명령.** 입력창에 **`/`만 쳐도** 명령 목록이 떠요.

**결론: 외울 건 없습니다. 그냥 한국어로 하고 싶은 걸 말하세요.**

---

## 👥 이 팀의 팀원(agents)

| 팀원 | 무슨 일을 하나요 |
|---|---|
| **pm-lead** (그룹장) | 요청을 받아 알맞은 팀원에게 나눠 주고, 현황 정합·쓰기 승인을 총괄 |
| **jira-analyst** | JQL로 현황을 읽어 진행 상태·통계·병목을 정리 (읽기만 함) |
| **backlog-groomer** | 백로그의 중복·우선순위·세분화·정보 누락을 진단하고 개선 제안 |
| **sprint-planner** | 용량·범위·목표를 계산해 다음 스프린트 계획을 만듦 |
| **issue-triager** | 신규·미분류 이슈를 타입·우선순위·라벨·담당 후보로 분류 제안 |
| **status-reporter** | 진행 현황·릴리스 보고서를 이해관계자용으로 작성 (읽기만 함) |
| **git-traceability** | GitHub(커밋·PR·브랜치)와 Jira(Epic/Story/Task)를 이슈키로 대조 — 커버리지·고아 커밋·이슈↔PR 매핑·Jira 갱신 초안 (읽기만 함) |
| **slack-capture** | Slack 채널·스레드를 읽어 액션아이템·블로커·결정사항을 캡처 → Jira 트리아지/이슈 초안 (읽기만 함) |
| **slack-notifier** | **유일하게 Slack에 게시하는 팀원.** 승인된 상태/번다운/블로커/릴리스 노트·알림만 채널에 발행 (게시 전 미리보기·승인) |
| **pm-reviewer** | **검수 담당.** 정합·실행가능성·승인 게이트 준수를 점검 (반영 전 거침) |
| **jira-writer** | **유일하게 Jira에 쓰는 팀원.** 승인된 변경만 실제로 반영 |
| **feedback-reporter** | 작업을 회고해 본부에 전달할 개선 피드백을 정리 (비밀값은 담지 않음) |

---

## ⌨️ 자주 쓰는 단축 명령(skills)

| 명령 | 이렇게 말하면 돼요 |
|---|---|
| `/jira-status` | "이 프로젝트 현황 알려줘 / 이슈 몇 건이야" |
| `/backlog-groom` | "백로그 정리해줘 / 중복·우선순위 봐줘" |
| `/sprint-plan` | "다음 스프린트 계획 짜줘 / 용량 계산해줘" |
| `/triage` | "신규 이슈 분류해줘 / 우선순위 매겨줘" |
| `/status-report` | "진행 현황 보고서 만들어줘 / 주간 보고" |
| `/trace` | "추적성 봐줘 / 이슈랑 PR 매핑 / 고아 커밋 찾아줘 / 이 Epic 어디까지 구현됐어" |
| `/slack-capture` | "이 슬랙 채널/스레드에서 할 일·블로커·결정 뽑아 Jira 초안 만들어줘" (읽기만) |
| `/slack-post` | "이 상태 리포트 슬랙 #채널에 올려줘 / 릴리스 노트 발행" (게시 전 미리보기·승인) |
| `/pm-review` | "이 계획 검수해줘 / 쓰기 계획 점검" |
| `/jira-apply` | "(승인한 변경) Jira에 반영해줘 / 이슈 만들어줘" |
| `/feedback-agents` | "이번 작업 회고해서 개선점 정리해줘" |

---

## 💬 이렇게 말해보세요 (예시)

- **"ABC 프로젝트 현재 진행 중인 이슈가 몇 건인지, 누가 막혀 있는지 알려줘."**
- **"이번 스프린트 끝났어. 백로그 정리해서 중복이랑 우선순위 봐줘."**
- **"다음 2주 스프린트 계획 짜줘. 가용 인원은 3명이야."**
- **"이번 주 들어온 미할당 버그들 트리아지해줘."**
- **"방금 만든 트리아지 결과 검수하고, 통과하면 Jira에 반영해줘."** (반영 전 변경 계획을 먼저 보여드려요)
- **"PROJ 프로젝트 ↔ 우리 GitHub 레포 추적성 봐줘. 이슈랑 PR이 어디까지 연결됐고 고아 커밋 있는지."** (`/trace`, 읽기만)
- **"머지된 PR 기준으로 Jira 갱신할 거 초안 만들어줘."** (초안만 만들고, 반영은 승인 게이트를 거쳐요)
- **"#standup 스레드에서 오늘 할 일·블로커·결정사항 뽑아서 Jira 초안 만들어줘."** (`/slack-capture`, 읽기만)
- **"이번 주 상태 보고서 #sprint-status 채널에 올려줘."** (`/slack-post` — 게시 전 미리보기를 먼저 보여드려요)

---

## 🚧 이 팀이 하지 않는 일 (다른 팀으로)

| 이런 게 필요하면 | 이 팀으로 |
|---|---|
| 기능명세·기술문서·기획 문서 | `docs_agents` |
| 실제 코드 구현·리뷰·디버그 | `dev_agents` |
| 테스트 전략·QA 실행 | `QA_agents` |
| 릴리스 노트·대외 공지 카피 | `marketing_agents` |

> 우리는 *Jira 위의 프로젝트 관리*를 합니다. 문서·코드·테스트·카피는 위 팀이 더 잘합니다.

---

## 🤝 다른 팀과 함께 쓰기 (멀티팀 조립)

> **에이전트만 복사해도 됩니다(자동 포트):** `대상/.claude/agents/<팀>/`에 에이전트만 복사하고 `/team-assemble`를
> 실행하면, 아래 수동 복사 단계(스킬·템플릿·참조 문서·`.mcp.json` 병합)는 스킬이 **본부 레포에서 자동으로
> 가져옵니다**(같은 머신에 본부 레포가 있을 때 — 경로는 1회만 물음, SKILL §2-1b). **환경변수 설정·브라우저 OAuth
> 1회 인증만 직접** 하면 됩니다. 본부 레포가 없는 환경에서는 아래 수동 절차를 그대로 따르세요.

이 PM팀을 **개발(dev)·QA·문서 등 다른 팀과 한 프로젝트에서 같이** 쓰고 싶을 때 읽으세요.
(PM팀만 혼자 쓸 거면 — 이 `pm_agents` 폴더를 그냥 직접 열면 됩니다. 조립은 필요 없어요.)

PM팀은 **폴더를 직접 여는 팀(모델 A)** 이라, 다른 팀과 합칠 땐 **에이전트(팀원)만 옮기면 부족**해요.
슬래시 명령(스킬)·문서 템플릿·Jira 연결 설정까지 함께 옮겨야 제대로 동작합니다.

### 무엇을 복사하나 (4가지)

대상 프로젝트(여러 팀을 같이 쓸 폴더)로 아래 4가지를 옮깁니다.

1. **① 팀원(에이전트):** `pm_agents/.claude/agents/*` → `대상-프로젝트/.claude/agents/pm/`
2. **② 단축 명령(스킬):** `pm_agents/.claude/skills/*` → `대상-프로젝트/.claude/skills/` (슬래시 명령이 동작하려면 필수)
3. **③ 문서 템플릿:** `pm_agents/templates/*` → `대상-프로젝트/templates/` (에이전트가 참조하는 상대경로 유지)
4. **④ MCP 연결 설정:** `pm_agents/.mcp.json` → `대상-프로젝트/.mcp.json`에 **합칩니다(병합)**. `atlassian`(Jira)·
   `github`(추적성, 읽기 전용)·`slack`(양방향) 세 서버가 들어 있어요. `atlassian`·`github`는 비밀값이 없고(OAuth),
   같은 키가 이미 있으면 사용자에게 확인합니다(특히 `github`는 dev팀과 동일 키라 충돌 시 하나로 합칩니다).
   ⚠️ **`slack`은 Confidential OAuth라 `${SLACK_CLIENT_ID}`/`${SLACK_CLIENT_SECRET}`(비밀값)** 이 필요해요 — `.mcp.json`엔
   환경변수 치환만 옮기고 **실제 secret은 대상 프로젝트에서도 레포 밖(환경변수/.env)으로** 주입합니다(커밋 금지).
   + 본부의 `team-assemble` 스킬 폴더도 대상 `.claude/skills/`로 함께 복사하세요.

> 회고용 `/feedback-agents`는 **따로 복사하지 않아도** `/team-assemble`이 자동으로 만들어 줍니다(위 도메인 스킬만 옮기면 돼요).
> `dev_agents`처럼 **복사해서 쓰는 팀(모델 B)** 은 `agents/*.md`만 옮기면 됩니다(스킬·템플릿·MCP 없음).

### 실행

대상 프로젝트에서 **`/team-assemble`를 한 번** 실행하면 됩니다.
→ 팀원 이름이 겹쳐도 자동으로 정리되고(충돌 해결), 협업용 `CLAUDE.md`·`MEMORY.md`와 통합 `/feedback-agents`가 자동으로 만들어집니다.
팀을 더하거나 빼면 **다시 실행만** 하면 됩니다(여러 번 돌려도 안전).

### 알아둘 점

- PM팀의 `CLAUDE.md`·`MEMORY.md`는 따라가지 않습니다 — 협업용 `CLAUDE.md`의 자동 블록과 공통 규칙으로 대체돼요.
  단, **쓰기 승인 게이트·프롬프트 인젝션 방어는 `jira-writer` 에이전트 본문에 자체 완결적으로** 박혀 있어 조립 후에도 그대로 동작합니다.
- 회고 담당(`feedback-reporter`)은 조립 시 **하나로 통합**됩니다. `/feedback-agents` 한 번이면 합쳐진 **모든 팀**을 회고해 본부로 보고됩니다.

### 🔌 MCP 주의 — 1회 재인증 (Jira · GitHub · Slack)

PM팀의 `.mcp.json`에는 **Atlassian(Jira)** · **GitHub(추적성, 읽기 전용)** · **Slack(양방향)** 공식 원격 MCP
설정이 들어 있어요. 대상 프로젝트로 병합한 뒤, **각 연결을 한 번 다시 인증**하면 됩니다.
- **Atlassian(Jira)** — `/mcp`에서 `atlassian` 서버 최초 호출 시 브라우저로 1회 로그인(OAuth, 비밀값 없음) → [`docs/ATLASSIAN-MCP.md`](docs/ATLASSIAN-MCP.md)
- **GitHub(추적성)** — `/trace`를 쓰면 `github` 서버 최초 호출 시 브라우저로 1회 OAuth(PAT 불필요, 읽기 전용) → [`docs/GITHUB-MCP.md`](docs/GITHUB-MCP.md)
  - dev팀과 **같은 `github` 키**라, dev팀과 함께 조립하면 GitHub 인증을 공유합니다(한 번만).
- **Slack(양방향)** — `/slack-capture`·`/slack-post`를 쓰면 `slack` 서버를 연결합니다. ⚠️ **Confidential OAuth라
  `client_id` + `client_secret`(비밀값)** 이 필요 — 대상 프로젝트에서도 `${SLACK_CLIENT_ID}`/`${SLACK_CLIENT_SECRET}`을
  **환경변수/.env로 주입(커밋 금지)** 한 뒤 `/mcp`로 연결합니다. 게시(외부 발신)는 항상 승인 후에만 → [`docs/SLACK-MCP.md`](docs/SLACK-MCP.md)

> 더 자세히는 본부의 루트 `README.md`/`CLAUDE.md`(§2-C 조립)와 `/team-assemble` 스킬을 보세요.

---
🤖 Built with [Claude Code](https://claude.com/claude-code)

---

## 🛠 고급 (개발자용)

여기부터는 팀을 깊이 이해하거나 직접 손보려는 분을 위한 내용입니다.

### 🔄 협업 흐름

```
입력(프로젝트/보드 · 요청)
   → pm-lead 분해
   → jira-analyst 현황(JQL · 조회 시점)
   → [backlog-groomer / sprint-planner / issue-triager / status-reporter] 가공(.md)
   → pm-reviewer 검수 게이트 (🔴 있으면 수정·재검수)
   → (변경 계획 표 → 사용자 승인) jira-writer 반영
   → pm-lead 통합 보고
```

자세한 단계는 `docs/WORKFLOW.md`, 검수 기준은 `docs/REVIEW-CHECKLIST.md`, 쓰기 게이트는 `docs/WRITE-GATE.md`.

### 🔌 MCP / 권한 (요약)

- **Atlassian 공식 원격 MCP**(`https://mcp.atlassian.com/v1/mcp`, Streamable HTTP·OAuth — 비밀값 없음. 구 `/v1/sse`는 2026-06-30 이후 지원 종료). 서버 키 `atlassian` → 도구명 `mcp__atlassian__*`.
- **읽기 도구는 모든 PM 에이전트**에, **쓰기 도구 5종은 `jira-writer`에만**(승인 게이트 통과 후). → `docs/ATLASSIAN-MCP.md`
- **GitHub 공식 원격 MCP**(`https://api.githubcopilot.com/mcp/`, `http`, OAuth — PAT 불필요, dev_agents와 동일). 서버 키 `github` → 도구명 `mcp__github__*`.
  **읽기 전용**으로 `git-traceability` 한 역할에만(추적성). GitHub 쓰기는 미부여(이번 범위 아님). git 출처 Jira 갱신도 동일 승인 게이트. → `docs/GITHUB-MCP.md`
- **Slack 공식 원격 MCP**(`https://mcp.slack.com/mcp`, `http`/Streamable HTTP, **Confidential OAuth — client_secret이 비밀값**). 서버 키 `slack` → 도구명 `mcp__slack__*`.
  **읽기는 `slack-capture`**(채널/스레드 캡처 → Jira 초안), **게시(외부 발신)는 `slack-notifier`만**(상태/번다운/블로커/릴리스 노트·알림 — HITL 승인 = Jira 쓰기와 동일 게이트). 쓰기 표면을 `jira-writer`와 별개로 격리. `create_conversation`(채널 생성) 1차 제외. `${SLACK_CLIENT_ID}`/`${SLACK_CLIENT_SECRET}` 환경변수 치환·비밀값 미커밋. → `docs/SLACK-MCP.md`
- **1차 범위: Jira.** Confluence는 읽기 보강만, JSM/Bitbucket/Compass는 1차 미와이어링(후속 확장).
- **보안:** 외부 MCP 도구 description/스키마 무결성 점검(도구 포이즈닝 방어) · 이슈/커밋/PR/**Slack 메시지** 본문 지시 자동 실행 금지(인젝션 방어 — Slack은 신뢰 불가 입력) · 비밀값(Atlassian 토큰·**Slack client_secret**) 레포 금지(`.gitignore`).

### 📂 폴더 구조

```
pm_agents/
├── CLAUDE.md            # 그룹장(오케스트레이터)
├── MEMORY.md            # 학습 누적층(@./MEMORY.md 자동 로드)
├── README.md           # (이 파일) 빠른 시작
├── .gitignore
├── .mcp.json           # Atlassian(Jira)·GitHub(추적성) OAuth + Slack(Confidential OAuth, ${ENV} 치환·비밀값 미커밋)
├── .claude/
│   ├── agents/         # 에이전트 12종 (자동 로드) — git-traceability·slack-capture·slack-notifier 포함
│   └── skills/         # 슬래시 스킬 11종 — /trace·/slack-capture·/slack-post 포함
├── templates/          # 산출물 템플릿 7종 — 추적성리포트.md·슬랙캡처.md 포함
├── feedback/           # 팀 작업 회고·피드백 보고서 (→ 본부) /feedback-agents
└── docs/               # 워크플로우 · MCP 가이드(Atlassian·GitHub·Slack) · 검수 체크리스트 · 쓰기 게이트
```

### 🧠 팀 메모리 (MEMORY.md) 사용법

이 팀은 두 층의 메모리를 쓴다.
- **CLAUDE.md** — 사람이 쓰는 안정적 규칙·라우팅(매 세션 전체 로드).
- **MEMORY.md** — 작업하며 쌓이는 학습(검증된 함정·우회법·환경 매핑·반복 교정). CLAUDE.md가 `@./MEMORY.md`로 자동 로드한다.

| 항목 | 내용 |
|---|---|
| **언제 적나** | 같은 실수 2회 · 환경/도구 특이사항(실제 노출 MCP 도구명·프로젝트키·보드ID) · 매 세션 다시 설명하게 되는 것 · 검수가 잡아준 것 |
| **어떻게** | 해당 섹션에 **한 줄**(가능하면 날짜·근거). 규칙으로 굳으면 CLAUDE.md로 승격 |
| **확인** | 세션 중 `/memory`로 로드된 메모리 파일 확인 |
| **용량** | 200줄 이하 유지. 초과 시 `memory/<주제>.md`로 분리(필요할 때만 읽힘) |
| **금지** | 비밀값·Atlassian 토큰은 적지 않는다. 개인·머신 한정 학습은 Claude Code 자동 메모리가 별도 관리(레포 비커밋) |

### 🧩 모델·프롬프트 기준 (Claude Opus 4.8)

이 팀은 현행 **Claude Opus 4.8**(별칭 `opus`, 1M 컨텍스트) 기준으로 운영한다. 라인업·근거·전체 표는 루트 `claude-opus-4.8-운영노트.md`.

- **모델 별칭 유지.** 에이전트 `model:`은 `opus`/`sonnet`/`haiku`/`inherit` 별칭(모델 ID 하드코딩 금지). 4.8는 천장이 높아 **effort는 `high`에서 시작**(반사적 `max` 금지).
- **담백한 지시.** 과장 명령("CRITICAL/반드시")은 과잉발동 → "…할 때 X" 형태로.
- **위임·도구·메모리는 트리거 명시.** `description`에 "~할 때 호출"을 박고, 그룹장 루프에 "작업 전 MEMORY 확인"을 둔다.
- **ask-rate·나레이션 보정.** 사소한 선택은 정하고 기록만(범위 변경·**Jira 쓰기**만 확인).
