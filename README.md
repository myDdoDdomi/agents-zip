# team-agents 🏢 — 부서별 에이전트 팀 본부

> 어떤 팀이든 가져다 **강력하게 쓸 수 있는 "부서 에이전트 팀"의 모음이자 공장**입니다.
> 각 부서(문서·QA·마케팅·백엔드·디자인 …)를 그룹장 + 역할별 서브에이전트로 구성한
> 독립 팀 폴더로 제공하고, **새 팀도 명령 한 줄로 찍어낼 수 있습니다.**

`anthropics/claude-code`의 서브에이전트·Skills·MCP를 토대로, 실제 조직의 직군 구조를
그대로 옮긴 에이전트 팀을 표준화했습니다. 설계 근거는
[`aiagents-deep-research-report.md`](aiagents-deep-research-report.md)(2026 Claude 에이전트 생태계 리서치)입니다.

---

## 🧭 구조 (2단 계층)

- **루트 [`CLAUDE.md`](CLAUDE.md) = 총괄 그룹장** — 새 팀을 *만들고*, 일을 알맞은 팀으로 *라우팅*.
- **각 `<부서>_agents/CLAUDE.md` = 부서 그룹장** — 그 부서 안에서 에이전트를 *지휘*.
- **[`.claude/agents/team-architect.md`] = 팀 빌더(팩토리)** — 새 부서 팀을 표준에 맞게 설계·생성.

---

## 👥 현재 부서 팀

| 부서 팀 | 무엇을 하나 | 배포 모델 | 사용법 |
|---|---|---|---|
| [`docs_agents`](docs_agents/) | 레퍼런스 리서치·인사이트 분석·기획·개발 문서 작성·검수·Google Drive/Sheets 반영·Calendar 일정·일정계획(간트·칸반) (16 에이전트 + 22 슬래시 스킬) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`QA_agents`](QA_agents/) | 테스트 전략·케이스 설계·자동화·API·성능·보안·접근성·모바일·회귀·트리아지 (13 에이전트) | B. 복사-라이브러리 | `agents/*.md`를 대상 프로젝트 `.claude/agents/`로 **복사** |
| [`design_agents`](design_agents/) | 아이데이션·디자인 토큰/시스템·Figma 추출·접근성·디자인-코드 정합 (7 에이전트 + 8 스킬, Figma 공식 MCP) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`dev_agents`](dev_agents/) | 아키텍처·구현·코드리뷰·디버그·리팩터·마이그레이션 + **인가된 침투 테스트(레드팀)→방어(블루팀) 퍼플팀 루프** (16 에이전트 + 3 스킬, GitHub+Context7 MCP) | B. 복사-라이브러리 | `agents/*.md`를 대상 프로젝트 `.claude/agents/`로 **복사** |
| [`marketing_agents`](marketing_agents/) | 캠페인·블로그·랜딩카피·SEO·소셜·이메일·포지셔닝 작성·검수 (8 에이전트 + 10 스킬, 빌트인 웹검색) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |
| [`resume_agents`](resume_agents/) | GitHub 경력·포트폴리오·지원 기업(JD) 분석을 조합한 기업 맞춤 이력서·포트폴리오 작성·검수·Drive 산출 (9 에이전트 + 9 스킬, Google Workspace(docs/drive)+GitHub 원격 MCP) | A. 자기완결형 | 폴더를 **Claude Code로 직접 연다** |

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
├── aiagents-deep-research-report.md  # 설계 근거 리서치 보고서
├── .claude/agents/team-architect.md  # 팀 빌더(팩토리)
├── docs_agents/                      # [부서 팀] 문서화 (모델 A)
├── QA_agents/                        # [부서 팀] QA (모델 B)
├── design_agents/                    # [부서 팀] 디자인 (모델 A)
├── dev_agents/                       # [부서 팀] 개발 (모델 B)
├── marketing_agents/                 # [부서 팀] 마케팅 (모델 A)
└── resume_agents/                    # [부서 팀] 이력서·포트폴리오 (모델 A)
```
> 각 `<부서>_agents/`에는 `CLAUDE.md`(그룹장) 옆에 `MEMORY.md`(학습 누적층)가 있고, `CLAUDE.md`가 `@./MEMORY.md`로 자동 로드합니다.

---
🤖 Built with [Claude Code](https://claude.com/claude-code)
