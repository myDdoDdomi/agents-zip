# resume-agents 📄 — 나만의 이력서·포트폴리오 AI 팀

내 **GitHub 경력**과 **포트폴리오**, 그리고 **지원하려는 회사·직무(채용공고)** 를 함께 분석해서, 그 회사에 딱 맞춘 **이력서와 포트폴리오**를 만들어 주고 **구글 문서(Google Docs/Drive)** 로 깔끔하게 저장까지 해 주는 AI 팀이에요. 한국어로 "토스 백엔드 직무용 이력서 만들어줘"처럼 말하기만 하면 됩니다.

> ⛔ **이 팀의 1순위 약속: 없는 경력·허위 스펙은 절대 지어내지 않아요.** 모든 경력·성과는 GitHub·포트폴리오·본인 확인 같은 근거에 기반하고, 근거가 없으면 `(확인 필요)`로 표시한 뒤 당신에게 직접 물어봅니다.

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

## 🚀 이 팀 쓰는 법 (3단계)

**① 이 폴더를 `claude`로 엽니다.**
터미널에서 이 `resume_agents` 폴더로 이동한 뒤 `claude` 라고 입력하세요. 그러면 이 팀의 팀원(에이전트)과 명령(스킬)이 자동으로 불러와집니다.

**② Google·GitHub 연결을 딱 1번만 해 둡니다.**
완성된 이력서를 구글 문서로 저장하고, GitHub에서 경력 근거를 읽어오기 위해 처음 한 번 연결이 필요해요. (안 해도 "붙여넣기" 방식으로 동작은 하지만, 연결해 두면 훨씬 편합니다.)
- **Google 문서·드라이브 연결** — 안내: [`docs/GOOGLE-WORKSPACE-MCP.md`](docs/GOOGLE-WORKSPACE-MCP.md) (구글 로그인 1회)
- **GitHub 연결** — 안내: [`docs/GITHUB-MCP.md`](docs/GITHUB-MCP.md) (GitHub은 **PAT**라는 읽기 전용 토큰을 한 번 넣어 줍니다. 일반 OAuth 로그인은 지원되지 않아요.)

**③ 한국어로 그냥 시키세요.**
예: **"토스 백엔드 직무용 이력서 만들어줘. 내 GitHub은 github.com/내아이디야."**
가장 편한 방법은 **`/tailor` 한 방**입니다. GitHub·포트폴리오·지원 회사만 알려주면 **근거 수집 → 맞춤 전략 → 작성 → 검수 → 구글 문서 저장**까지 처음부터 끝까지 알아서 해 줍니다.

```
/tailor  GitHub: github.com/내아이디  포트폴리오: https://my.site  지원: 토스 / 백엔드
```

---

## 🤖 "에이전트"와 "스킬"이 뭐예요? (1분 개념)

어렵지 않아요. 딱 두 가지만 알면 됩니다.

- **에이전트(agents) = AI 팀원.** 각자 한 분야 전문가예요. 당신이 한국어로 시키면 **그룹장이 알맞은 팀원에게 알아서 맡깁니다.** → *팀원 이름을 외울 필요가 없어요.*
- **스킬(skills) = `/`로 시작하는 단축 명령.** 그냥 말로 시켜도 되고, 콕 집어 부를 수도 있어요. 입력창에 **`/`만 쳐도 쓸 수 있는 명령 목록**이 뜹니다.

**결론: 외울 건 없습니다. 그냥 한국어로 하고 싶은 걸 말하세요.** 아래 표는 "이 팀이 뭘 해줄 수 있는지" 참고용이에요.

---

## 👥 이 팀의 팀원(agents)

| 팀원 | 무슨 일을 하나요 |
|---|---|
| **resume-lead** (그룹장) | 당신의 요청을 받아 알맞은 팀원에게 일을 나눠 주고, "사실인지·회사에 잘 맞는지" 최종 점검하는 총괄 |
| **github-career-extractor** | 당신의 GitHub 레포·기여·기술스택을 읽어 **경력 근거**로 정리 (읽기만 함) |
| **portfolio-analyzer** | 포트폴리오 사이트 주소(URL)를 읽어 프로젝트·역량·성과를 뽑아냄 |
| **company-job-researcher** | 지원할 회사와 채용공고(JD)를 분석해 인재상·기술스택·핵심 키워드를 도출 |
| **resume-writer** | 회사에 맞춘 **이력서 초안**을 작성 (채용공고 키워드 정렬·성과 중심) |
| **portfolio-writer** | 회사에 맞춘 **포트폴리오 문서**를 작성 (프로젝트 이야기·성과 중심) |
| **resume-reviewer** | **검수 담당.** 사실인지·과장은 없는지·회사에 잘 맞는지 꼼꼼히 점검 (저장 전 반드시 거침) |
| **docs-formatter** | 완성본을 보기 좋은 **구글 문서 서식**으로 만들어 드라이브에 저장·정리 |
| **feedback-reporter** | 작업을 회고해 본부에 전달할 개선 피드백을 정리 (개인정보는 담지 않음) |

---

## ⌨️ 자주 쓰는 단축 명령(skills)

표를 외울 필요는 없어요. 오른쪽처럼 말하면 알아서 동작합니다. (입력창에 `/`만 쳐도 목록이 떠요.)

| 명령 | 이렇게 말하면 돼요 |
|---|---|
| `/tailor` | "전체 다 만들어줘 / 이 회사 지원 준비 처음부터 끝까지" (가장 추천 — 한 방에 완성) |
| `/github-career` | "내 깃허브 경력 정리해줘 / 레포 분석해줘" |
| `/portfolio-analyze` | "내 포트폴리오 사이트 읽고 프로젝트 뽑아줘" |
| `/company-research` | "이 회사·채용공고 분석해줘 / JD 핵심 키워드 알려줘" |
| `/resume-build` | "이 회사용 이력서 만들어줘" |
| `/portfolio` | "이 회사용 포트폴리오 문서 만들어줘" |
| `/resume-review` | "이력서 검수해줘 / 과장이나 틀린 거 없는지 봐줘" |
| `/drive-sync` | "완성본 구글 문서로 저장해줘 / 드라이브에 올려줘" |
| `/feedback-agents` | "이번 작업 회고해서 개선점 정리해줘" |

---

## 💬 이렇게 말해보세요 (예시)

- **"토스 백엔드 직무용 이력서 만들어줘. GitHub은 github.com/내아이디, 포트폴리오는 https://my.site 야."**
- **"내 깃허브 보고 경력 근거부터 정리해줘."**
- **"이 채용공고 분석해서 어떤 키워드를 강조하면 좋을지 알려줘."** (회사명·직무·공고 내용 같이 알려주면 더 정확해요)
- **"방금 만든 이력서 과장된 표현 없는지 검수하고, 통과하면 구글 문서로 저장해줘."**

> 💡 회사명과 직무, 그리고 가능하면 **채용공고 원문/링크**를 함께 주면 훨씬 더 잘 맞춘 이력서가 나옵니다. 같은 경력이라도 회사마다 강조할 부분이 다르거든요.

---

## 🚧 이 팀이 하지 않는 일 (다른 팀으로)

| 이런 게 필요하면 | 이 팀으로 |
|---|---|
| 자기소개서·블로그 등 **범용 마케팅 카피** | `marketing_agents` |
| 포트폴리오 **사이트/코드를 직접 구현·수정** | `dev_agents` |
| 포트폴리오 **비주얼·디자인** | `design_agents` |

> 우리는 *경력 근거 → 회사 맞춤 이력서·포트폴리오 문서*까지 만듭니다. 코드 구현·비주얼·범용 카피는 위 팀이 더 잘합니다.

---

## 🤝 다른 팀과 함께 쓰기 (멀티팀 조립)

> **에이전트만 복사해도 됩니다(자동 포트):** `대상/.claude/agents/<팀>/`에 에이전트만 복사하고 `/team-assemble`를
> 실행하면, 아래 수동 복사 단계(스킬·템플릿·참조 문서·`.mcp.json` 병합)는 스킬이 **본부 레포에서 자동으로
> 가져옵니다**(같은 머신에 본부 레포가 있을 때 — 경로는 1회만 물음, SKILL §2-1b). **환경변수 설정·브라우저 OAuth
> 1회 인증만 직접** 하면 됩니다. 본부 레포가 없는 환경에서는 아래 수동 절차를 그대로 따르세요.

이 이력서팀을 **개발(dev)·디자인·QA 등 다른 팀과 한 프로젝트에서 같이** 쓰고 싶을 때 읽으세요.
(이력서팀만 혼자 쓸 거면 — 이 `resume_agents` 폴더를 그냥 직접 열면 됩니다. 조립은 필요 없어요.)

이력서팀은 **폴더를 직접 여는 팀(모델 A)** 이라, 다른 팀과 합칠 땐 **에이전트(팀원)만 옮기면 부족**해요.
슬래시 명령(스킬)·문서 템플릿·구글/깃허브 연결 설정까지 함께 옮겨야 제대로 동작합니다.

### 무엇을 복사하나 (4가지)

대상 프로젝트(여러 팀을 같이 쓸 폴더)로 아래 4가지를 옮깁니다.

1. **① 팀원(에이전트):** `resume_agents/.claude/agents/*` → `대상-프로젝트/.claude/agents/resume/`
2. **② 단축 명령(스킬):** `resume_agents/.claude/skills/*` → `대상-프로젝트/.claude/skills/` (슬래시 명령이 동작하려면 필수)
3. **③ 문서 템플릿:** `resume_agents/templates/*` → `대상-프로젝트/templates/` (에이전트가 참조하는 상대경로를 유지)
4. **④ 구글·깃허브 연결 설정:** `resume_agents/.mcp.json` → `대상-프로젝트/.mcp.json`에 **합칩니다(병합)**.
   비밀값은 들어 있지 않고 `${ENV}` 형태로 두며, 같은 키가 이미 있으면 사용자에게 확인합니다.
   + 본부의 `team-assemble` 스킬 폴더도 대상 `.claude/skills/`로 함께 복사하세요.

> 회고용 `/feedback-agents`는 **따로 복사하지 않아도** `/team-assemble`이 자동으로 만들어 줍니다(위 도메인 스킬만 옮기면 돼요).
> `dev_agents`처럼 **복사해서 쓰는 팀(모델 B)** 은 `agents/*.md`만 옮기면 됩니다(스킬·템플릿·MCP 없음).

### 실행

대상 프로젝트에서 **`/team-assemble`를 한 번** 실행하면 됩니다.
→ 팀원 이름이 겹쳐도 자동으로 정리되고(충돌 해결), 협업용 `CLAUDE.md`·`MEMORY.md`와 통합 `/feedback-agents`가 자동으로 만들어집니다.
팀을 더하거나 빼면 **다시 실행만** 하면 됩니다(여러 번 돌려도 안전).

### 알아둘 점

- 이력서팀의 `CLAUDE.md`·`MEMORY.md`는 따라가지 않습니다 — 협업용 `CLAUDE.md`의 자동 블록과 공통 규칙으로 대체돼요.
- 회고 담당(`feedback-reporter`)은 조립 시 **하나로 통합**됩니다. `/feedback-agents` 한 번이면 합쳐진 **모든 팀**을 회고해 본부로 보고됩니다.

### 🔌 구글·깃허브(MCP) 주의 — 각 1회 재인증

이력서팀의 `.mcp.json`에는 **Google Workspace(Docs·Drive)** 와 **GitHub 원격 MCP** 연결 설정이 들어 있어요.
대상 프로젝트로 병합한 뒤, **각 연결을 한 번씩 다시 인증**하면 됩니다(비밀값은 레포에 안 들어가요).
- **Google 문서·드라이브** — 본인 구글 계정으로 브라우저에서 1회 로그인 → [`docs/GOOGLE-WORKSPACE-MCP.md`](docs/GOOGLE-WORKSPACE-MCP.md)
- **GitHub** — 읽기 전용 PAT(토큰)를 `GITHUB_PAT` 환경변수에 1회 넣기(OAuth 로그인은 미지원) → [`docs/GITHUB-MCP.md`](docs/GITHUB-MCP.md)

> 더 자세히는 본부의 루트 `README.md`/`CLAUDE.md`(§2-C 조립)와 `/team-assemble` 스킬을 보세요.

---
🤖 Built with [Claude Code](https://claude.com/claude-code)

---

## 🛠 고급 (개발자용)

여기부터는 팀을 깊이 이해하거나 직접 손보려는 분을 위한 내용입니다. 일반 사용자는 위 내용만으로 충분해요.

### 🔄 협업 흐름

```
입력(GitHub·포트폴리오·기업/JD)
   → resume-lead 분해
   → [github-career-extractor / portfolio-analyzer / company-job-researcher] 병렬 근거 수집
   → resume-writer / portfolio-writer 맞춤 초안 (JD 정렬·STAR)
   → resume-reviewer 검수 게이트 (🔴 있으면 수정·재검수)
   → docs-formatter (Docs 서식 → Drive 저장)
   → resume-lead 통합 보고
```

자세한 단계는 `docs/WORKFLOW.md`, 검수 기준은 `docs/REVIEW-CHECKLIST.md`, 서식은 `docs/DOC-FORMATTING.md`.

### 🔌 MCP / 권한 (요약)

- **Google Workspace**: `docs`·`drive` 도구만 활성화(권한 표면 최소화). 쓰기 가능 → 덮어쓰기 전 사용자 확인.
- **GitHub**: 공식 원격 MCP, **읽기 전용** 권장(경력 근거). fine-grained PAT를 `GITHUB_PAT` 환경변수로 주입(Contents/Metadata=Read-only). 이 엔드포인트는 자동 OAuth(DCR) 미지원 — PAT가 비면 `/mcp`에서 HTTP 400.
- 기업·직무 조사: 빌트인 **WebSearch/WebFetch**(별도 MCP 없음).
- **비밀값은 레포 금지** — `${ENV}` 치환 + `.gitignore`(client_secret·token·.env 류 차단).

### 📂 폴더 구조

```
resume_agents/
├── CLAUDE.md            # 그룹장(오케스트레이터)
├── README.md           # (이 파일) 빠른 시작
├── .gitignore
├── .mcp.json           # Google Workspace(docs/drive) + GitHub 원격 MCP
├── .claude/
│   ├── agents/         # 에이전트 9종 (자동 로드)
│   └── skills/         # 슬래시 스킬 9종
├── templates/          # 산출물 템플릿 5종
├── feedback/           # 팀 작업 회고·피드백 보고서 (→ 본부) /feedback-agents
└── docs/               # 워크플로우 · 서식 · 검수 체크리스트 · MCP 가이드
```

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

### 🧩 모델·프롬프트 기준 (Claude Opus 4.8)

이 팀은 현행 **Claude Opus 4.8**(별칭 `opus`, 1M 컨텍스트) 기준으로 운영한다. 라인업·근거·전체 표는 루트 `claude-opus-4.8-운영노트.md`.

- **모델 별칭 유지.** 에이전트 `model:`은 `opus`/`sonnet`/`haiku`/`inherit` 별칭으로 둔다(모델 ID 하드코딩 금지 — Claude Code가 현행 버전으로 해석). 4.8는 천장이 높아 **effort는 `high`에서 시작**, 필요 시 상향(반사적 `max` 금지).
- **담백한 지시.** 4.8는 지시를 문자 그대로 따른다 → "CRITICAL/반드시/무조건" 같은 과장 명령은 과잉발동을 부르니 "…할 때 X" 형태로.
- **위임·도구·메모리는 트리거 명시.** 4.8는 위임/검색/메모리를 보수적으로 쓴다 → `description`과 도구 설명에 **"~할 때 호출"** 을 박고, 그룹장 루프에 "작업 전 MEMORY 확인"을 둔다.
- **ask-rate·나레이션 보정.** 사소한 선택은 정하고 기록만(범위 변경·파괴적 작업·Drive 산출만 확인). 도구 호출 사이 불필요한 나레이션은 줄인다.
