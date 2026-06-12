# team-agents 🏢 — 부서별 에이전트 팀 본부

> 어떤 팀이든 가져다 **강력하게 쓸 수 있는 "부서 에이전트 팀"의 모음이자 공장**입니다.
> 각 부서(문서·QA·마케팅·백엔드·디자인 …)를 그룹장 + 역할별 서브에이전트로 구성한
> 독립 팀 폴더로 제공하고, **새 팀도 명령 한 줄로 찍어낼 수 있습니다.**

`anthropics/claude-code`의 서브에이전트·Skills·MCP를 토대로, 실제 조직의 직군 구조를
그대로 옮긴 에이전트 팀을 표준화했습니다. 설계 근거는
[`aiagents-deep-research-report.md`](aiagents-deep-research-report.md)(2026 Claude 에이전트 생태계 리서치)입니다.

---

## 🟢 Claude Code 처음이세요? — 설치 1분

이 팀들은 **Claude Code**(터미널에서 도는 AI 도구) 위에서 동작합니다. 처음이면 아래 한 줄만 따라 하세요.

터미널(명령을 입력하는 검은 창)을 열고 **자기 컴퓨터에 맞는 아래 한 줄을 복사 → 붙여넣기 → Enter** 하면 끝납니다.

**🍎 macOS** — `Terminal(터미널)` 앱 실행 후:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**🪟 Windows** — 시작 메뉴에서 `PowerShell` 검색해 실행 후:

```powershell
irm https://claude.ai/install.ps1 | iex
```

설치가 끝나면 같은 창에 **`claude`** 라고 입력하고 Enter → 처음 한 번만 **브라우저로 로그인**(Claude Pro/Max/Team 또는 Console 계정)하면 됩니다. 그다음부터는 `claude`만 치면 바로 시작.

> 막히면 공식 설치 가이드: <https://code.claude.com/docs/en/quickstart> · Windows는 `Git for Windows`가 깔려 있으면 더 매끄럽습니다.

**그다음은?** 위 표의 팀 폴더를 골라 그 폴더의 `README.md`를 보세요 — 비개발자용 사용법(에이전트·스킬 소개 포함)이 들어 있습니다.
> 한 줄 개념: **에이전트 = AI 팀원**(시키면 그룹장이 알맞은 팀원에게 맡김) · **스킬 = `/`로 시작하는 단축 명령**(입력창에 `/`만 쳐도 목록이 뜸). **외울 것 없이 한국어로 말하면 됩니다.**

---

## 🧭 구조 (2단 계층)

- **루트 [`CLAUDE.md`](CLAUDE.md) = 총괄 그룹장** — 새 팀을 *만들고*, 일을 알맞은 팀으로 *라우팅*.
- **각 `<부서>_agents/CLAUDE.md` = 부서 그룹장** — 그 부서 안에서 에이전트를 *지휘*.
- **[`.claude/agents/team-architect.md`] = 팀 빌더(팩토리)** — 새 부서 팀을 표준에 맞게 설계·생성.

**본부 유틸리티(부서 팀 아님):**
- **`/update-agents`** — IT 트렌드를 리서치해 관련 팀을 디벨롭하는 자기개선 파이프라인(`trend-researcher`→`update-curator`→`team-fit-reviewer` 팬아웃→사람 승인 게이트→`team-architect` 적용 → `.claude/skills/update-agents/runs/`에 기록).
- **`/tech-briefing`** — 사람이 읽는 IT 뉴스 종합 브리핑 발행(팀 수정 없음 → `briefings/`).
- **`/team-assemble`** — 여러 팀을 한 프로젝트에 조립(아래 *멀티팀 조립* 참고).
- 본부도 **2계층 메모리**를 운영합니다(루트 `MEMORY.md` = 자기개선 학습 누적층).

---

## 👥 현재 부서 팀

| 부서 팀 | 무엇을 하나 | 배포 모델 | 사용법 |
|---|---|---|---|
| [`docs_agents`](docs_agents/) | 레퍼런스 리서치·인사이트 분석·기획·개발 문서(기능/기술명세 포함) 작성·검수·Google Drive/Sheets 반영(서식=XLSX 빌드→import)·Calendar 일정·일정계획(간트·칸반)·요구 인터뷰·다관점 심화검수·Diátaxis (17 에이전트 + 24 슬래시 스킬) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`QA_agents`](QA_agents/) | 테스트 전략·케이스 설계·자동화·API·성능·보안·접근성·모바일·회귀·트리아지·**디자인 정합·비주얼 리그레션** (14 에이전트) | B. 복사-라이브러리 | `agents/*.md`를 대상 프로젝트 `.claude/agents/`로 **복사** |
| [`design_agents`](design_agents/) | 디자인 레퍼런스·트렌드 리서치(구현 레지스트리→dev 핸드오프 포함)·아이데이션·디자인 토큰/시스템·Figma 추출·접근성·디자인-코드 정합 (8 에이전트 + 9 스킬, Figma 공식 MCP) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`dev_agents`](dev_agents/) | 아키텍처·구현·코드리뷰·디버그·리팩터·마이그레이션 + **인가된 침투 테스트(레드팀)→방어(블루팀) 퍼플팀 루프** (16 에이전트 + 3 스킬, GitHub+Context7 MCP) | B. 복사-라이브러리 | `agents/*.md`를 대상 프로젝트 `.claude/agents/`로 **복사** |
| [`marketing_agents`](marketing_agents/) | 캠페인·블로그·랜딩카피·SEO(GEO·AEO 포함)·소셜·이메일·포지셔닝 작성·검수 (8 에이전트 + 10 스킬, 빌트인 웹검색) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`resume_agents`](resume_agents/) | GitHub 경력·포트폴리오·지원 기업(JD) 분석을 조합한 기업 맞춤 이력서·포트폴리오 작성·검수·Drive 산출 (9 에이전트 + 9 스킬, Google Workspace(docs/drive)+GitHub 원격 MCP) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`aiot_agents`](aiot_agents/) | AIoT(IIoT·스마트홈·웨어러블·로보틱스) 기획·요구분석·시스템 아키텍처·프로토콜·엣지AI·펌웨어·클라우드 데이터 설계+참조 구현·디바이스 보안 위협모델(STRIDE)·검수 (9 에이전트 + 6 스킬, Context7 read-only MCP) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`pm_agents`](pm_agents/) | Jira(Atlassian) 프로젝트 관리 — 현황분석·백로그·스프린트·트리아지·보고 + **쓰기는 승인 게이트(HITL) 통과 후에만** + GitHub 트레이서빌리티 + Slack 양방향(게시=HITL) (12 에이전트 + 11 스킬, Atlassian+GitHub+Slack 공식 원격 MCP) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| `edu_agents` ※별도 repo | IT·개발 교육 교안 — 학습설계(Bloom)·본문·실습·퀴즈·도식 + 인쇄/PDF HTML 빌드·검수 (9 에이전트 + 11 스킬) | A. 자기완결형 | 별도 repo 관리(본부 레포 비커밋) |
| `webnovel_agents` ※비커밋 | 인터뷰 기반 웹소설 창작 — 바이블·세계관·스토리나침반·인물 파일·복선원장·집필·윤문·검수·베타리드 (10 에이전트 + 13 스킬) | A. 자기완결형 | 원고=개인 자산(본부 레포 비커밋) |

> **배포 모델 A** = 자기 산출물을 만드는 팀(직접 열어 사용). **B** = 기존 코드베이스 위에서 일하는 팀(복사해 사용).
> **모든 팀**은 작업을 회고해 본부에 전달하는 피드백 보고 기능(`/feedback-agents` + `feedback-reporter` + `feedback/`)을 기본 탑재합니다(위 카운트 포함).
> **모든 팀**은 **2계층 메모리**(`CLAUDE.md` 안정 규칙 + `MEMORY.md` 누적 학습)를 기본 탑재합니다 — 그룹장 CLAUDE.md가 `@./MEMORY.md`로 자동 로드(아래 *2계층 메모리* 참고).

---

## 🚀 새 부서 팀 만들기

이 레포를 Claude Code로 열고 자연어로 요청하면 됩니다.

```
> 마케팅 팀 에이전트 만들어줘
> 백엔드 에이전트 팀 구성해줘
```

→ 총괄 그룹장이 `team-architect`(팀 빌더)에게 위임 → **설계안을 먼저 제시 → 승인하면**
폴더·그룹장(CLAUDE.md)·역할별 에이전트·(필요 시)스킬·README를 표준에 맞춰 생성합니다.

생성 시 `team-architect`가 자동으로 판단/적용하는 것:
- 팀 성격에 맞는 **배포 모델(A/B)** 선택
- 실제 직군에 맞는 **역할별 서브에이전트** + **최소 권한 도구** + **모델 배분**(opus/sonnet/haiku)
- 영역별 권장 **MCP**(예: 디자인=Figma, 프론트=Playwright, 백엔드=Supabase/K8s/Terraform)와 **권한·리스크** 고지

---

## 🤝 여러 팀 함께 쓰기 (멀티팀 조립 · `/team-assemble`)

한 팀이 아니라 **여러 팀을 한 프로젝트에서 같이** 쓰고 싶을 때(예: **dev + QA**, **design + dev**),
각 팀 에이전트를 `.claude/agents/` **팀별 하위폴더**로 정리하고 `/team-assemble` 스킬을 실행하면
**협업용 `CLAUDE.md`를 자동 생성/갱신**해 줍니다.

**1) 팀별 하위폴더로 복사**
```
대상-프로젝트/.claude/
├── agents/
│   ├── dev/      ← dev_agents 의 에이전트 *.md 복사
│   ├── qa/       ← QA_agents 의 에이전트 *.md 복사
│   └── design/   ← design_agents 의 에이전트 *.md 복사
├── skills/team-assemble/   ← 이 레포의 스킬 폴더 복사
└── CLAUDE.md     ← /team-assemble 이 생성/갱신
```

**2) 스킬 실행**
```
> /team-assemble
```
→ `agents/<팀>/` 폴더를 스캔해 ① **이름 충돌 자동 해결**(두 팀이 같은 `code-reviewer`를 가질 때처럼 겹치는 이름을
`dev-code-reviewer`·`qa-code-reviewer`로 리네임. 단 공통 `feedback-reporter`는 **단일 통합 reporter로 수렴** —
`/feedback-agents` 한 번으로 전 팀을 회고해 본부에 **통합 보고**) ② **모델 A 팀의 동반 자산 자동 포트**(아래)
③ **멀티팀 오케스트레이터 `CLAUDE.md`** 생성(팀 인벤토리·통합 라우팅표·**팀 간 핸드오프**: 예 `design → dev → qa → dev`).
팀을 더하거나 빼면 **다시 실행만** 하면 됩니다(재실행 안전).

> **모델 A 팀(docs·design·pm 등)도 에이전트만 복사하면 됩니다** — `/team-assemble`가 그 팀의 **스킬·`templates/`·
> 참조 문서(`docs/`·`config/`)·`.mcp.json` 병합**을 본부 레포에서 **자동으로 가져옵니다**(같은 머신에 본부 레포가
> 있을 때, 경로는 1회만 물음 — SKILL §2-1b). **환경변수 설정·브라우저 OAuth 1회 인증만 수동**입니다(필요 목록은
> 실행 후 보고에 출력). 본부 레포가 없는 환경이면 수동 복사 안내가 나옵니다. 모델 B 팀(dev·QA)은 원래 `agents/*.md`만 복사하면 됩니다.

> **팀 업데이트 재배포 시:** 업데이트된 에이전트를 기존 조립 프로젝트에 다시 넣을 땐 ① 팀별 폴더에만 복사
> ② 협업 `CLAUDE.md`는 절대 통째 덮어쓰지 않기 ③ 동명 파일은 diff 확인 후 교체 ④ `/team-assemble` 재실행 —
> 4가지를 지키세요(SKILL §1-2 체크리스트, 실제 오배치·덮어쓰기 사고 재발 방지).

> **왜 필요한가:** Claude Code는 `.claude/agents/` 하위폴더를 재귀 인식하지만 에이전트 **식별자는 `name`뿐**이라
> 폴더가 달라도 **같은 이름은 경고 없이 하나가 사라집니다.** `/team-assemble`이 이 충돌을 풀고 협업 라우팅을 자동으로 세팅합니다.

---

## 🧱 전 팀 공통 표준

모든 팀이 상속하는 원칙(상세: [`CLAUDE.md`](CLAUDE.md)):

- **한국어 우선** · **공식 스택 우선**(서브에이전트+Skills+MCP) · **그룹장=오케스트레이터**
- **역할=권한(최소 권한)** · **근거·투명성** · **Human-in-the-loop** · **비밀값 레포 금지**
- **효율 운영**: 작업 깊이 3단(빠르게/표준/심화) + 낭비 제거(원본 1회 요약 공유 등)
- **피드백 보고 기본 탑재**: 모든 팀이 `/feedback-agents`로 작업을 회고해 본부에 개선 피드백을 산출(피드백 루프)
- **2계층 메모리 기본 탑재**: `CLAUDE.md`(사람이 쓰는 안정적 규칙) + `MEMORY.md`(작업하며 쌓이는 학습)

### 🧠 2계층 메모리 (CLAUDE.md + MEMORY.md)

각 팀은 두 층의 메모리를 씁니다.

| 층 | 파일 | 누가 쓰나 | 무엇을 | 로드 |
|---|---|---|---|---|
| 규칙층 | `CLAUDE.md` | 사람 | 안정적 규칙·라우팅 | 매 세션 전체 자동 로드 |
| 학습층 | `MEMORY.md` | 사람/에이전트 | 검증된 함정·우회법·환경 매핑·반복 교정 | `CLAUDE.md`의 `@./MEMORY.md`로 자동 로드 |

- **적는 법:** 같은 실수를 두 번 했거나, 환경/도구 특이사항을 알게 됐거나, 매 세션 다시 설명하게 되는 게 생기면 해당 팀 `MEMORY.md`에 **한 줄**(가능하면 날짜·근거) 추가. 규칙으로 굳으면 `CLAUDE.md`로 승격.
- **용량:** `MEMORY.md`는 **200줄 이하** 유지(초과 시 `memory/<주제>.md`로 분리 — 필요할 때만 읽힘).
- **확인:** 세션 중 `/memory`로 로드된 메모리 파일을 확인.
- **개인 vs 팀:** 개인·머신 한정 학습은 Claude Code **자동 메모리**(`~/.claude/projects/.../memory/`)가 별도 관리(레포에 커밋 안 됨). **팀이 공유**하는 학습은 커밋되는 `MEMORY.md`에.
- **왜 `@import`?** Claude Code는 `CLAUDE.md`만 자동 로드하므로, 옆에 둔 `MEMORY.md`는 `@./MEMORY.md` import가 있어야 로드됩니다.

---

## 📂 폴더 구조

```
team-agents/
├── CLAUDE.md                         # 총괄 그룹장 (오케스트레이터)
├── README.md                         # (이 파일)
├── MEMORY.md                         # 본부 학습 누적층 (자기개선)
├── aiagents-deep-research-report.md  # 설계 근거 리서치 보고서
├── claude-opus-4.8-운영노트.md       # 현행 모델 기준 (라인업·effort·프롬프트)
├── .claude/
│   ├── agents/                       # 본부 전담: team-architect(팀 빌더) + trend-researcher·
│   │                                 #   update-curator·team-fit-reviewer·tech-news-reporter
│   └── skills/                       # /team-assemble · /update-agents(+runs/) · /tech-briefing
├── briefings/                        # IT 뉴스 브리핑 발행물 (/tech-briefing)
├── feedback/                         # 각 팀 → 본부 피드백 보고서 (피드백 루프)
├── docs_agents/                      # [부서 팀] 문서화 (모델 A)
├── QA_agents/                        # [부서 팀] QA (모델 B)
├── design_agents/                    # [부서 팀] 디자인 (모델 A)
├── dev_agents/                       # [부서 팀] 개발 (모델 B)
├── marketing_agents/                 # [부서 팀] 마케팅 (모델 A)
├── resume_agents/                    # [부서 팀] 이력서·포트폴리오 (모델 A)
├── aiot_agents/                      # [부서 팀] AIoT 기획·설계·개발 (모델 A)
├── pm_agents/                        # [부서 팀] Jira 프로젝트 관리 (모델 A)
├── edu_agents/                       # [부서 팀] 교육 교안 (모델 A) — 별도 repo·비커밋
└── webnovel_agents/                  # [부서 팀] 웹소설 창작 (모델 A) — 비커밋(원고=개인 자산)
```
> 각 `<부서>_agents/`에는 `CLAUDE.md`(그룹장) 옆에 `MEMORY.md`(학습 누적층)가 있고, `CLAUDE.md`가 `@./MEMORY.md`로 자동 로드합니다.

---
🤖 Built with [Claude Code](https://claude.com/claude-code)
