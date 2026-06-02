# resume-agents 📄

> 구직자의 **GitHub 경력 + 포트폴리오 + 지원 기업/직무(JD)** 를 분석·조합해, 그 기업에
> **맞춤화된 이력서·포트폴리오**를 빠르게 만들고 **Google Docs/Drive**로 산출하는 8명의 에이전트 팀입니다.

이 팀은 **모델 A(자기완결형)** — 이 폴더를 Claude Code로 **직접 열어** 슬래시 스킬·템플릿을 자동
로드해 씁니다. 외부 MCP 2개(Google Workspace Docs/Drive · GitHub 원격)를 사용하며, 기업·직무
조사는 빌트인 웹검색을 씁니다.

> ⛔ **이 팀의 1순위 원칙: 추측 경력·허위 스펙 절대 금지.** 모든 경력·성과·스킬은 근거(GitHub·
> 포트폴리오·사용자 확인)에 기반하고, 근거가 없으면 `(확인 필요)`로 막고 사용자에게 묻습니다.

---

## 🚀 빠른 시작

1. **이 폴더를 Claude Code로 엽니다.**
2. (1회) MCP 인증:
   - **Google Workspace(Docs/Drive)** — `docs/GOOGLE-WORKSPACE-MCP.md` (Google OAuth, client_secret 경로 환경변수)
   - **GitHub(경력 근거 읽기)** — `docs/GITHUB-MCP.md` (fine-grained PAT를 `GITHUB_PAT` 환경변수로 주입, 읽기 전용 권장 / 자동 OAuth 미지원)
   - MCP를 아직 못 써도 폴백(붙여넣기)으로 동작합니다.
3. 한국어로 시킵니다. 가장 쉬운 시작은 `/tailor`:
   ```
   /tailor  GitHub: github.com/내아이디  포트폴리오: https://my.site  지원: 토스 / 프론트엔드
   ```
   → 근거 수집(병렬) → 맞춤 전략 → 작성 → 검수 게이트 → Drive 반영을 한 번에 진행합니다.

---

## 👥 팀 구성 (8 에이전트)

| 에이전트 | 역할 | model |
|---|---|---|
| [`resume-lead`](.claude/agents/resume-lead.md) | 입력 분해·맞춤 전략·통합·게이트 판정(오케스트레이터) | opus |
| [`github-career-extractor`](.claude/agents/github-career-extractor.md) | GitHub 레포·기여·스택을 경력 근거로 추출(GitHub MCP, 읽기 전용) | sonnet |
| [`portfolio-analyzer`](.claude/agents/portfolio-analyzer.md) | 포트폴리오 URL → 프로젝트·역량·성과 추출(read-only) | sonnet |
| [`company-job-researcher`](.claude/agents/company-job-researcher.md) | 지원 기업·JD 분석(인재상·스택·키워드, read-only) | opus |
| [`resume-writer`](.claude/agents/resume-writer.md) | 기업 맞춤 이력서 초안(JD 정렬·STAR) | sonnet |
| [`portfolio-writer`](.claude/agents/portfolio-writer.md) | 기업 맞춤 포트폴리오 문서(프로젝트 서사·임팩트) | sonnet |
| [`resume-reviewer`](.claude/agents/resume-reviewer.md) | 검수 게이트: 사실·적합도·과장/금칙(read-only) | opus |
| [`docs-formatter`](.claude/agents/docs-formatter.md) | Google Docs 서식 + Drive 저장·정리 | haiku |

## 🧩 슬래시 스킬 (8)

| 스킬 | 담당 | 산출물 |
|---|---|---|
| `/tailor` | resume-lead | 처음부터 끝까지 맞춤(오케스트레이션) |
| `/github-career` | github-career-extractor | GitHub 경력 근거 |
| `/portfolio-analyze` | portfolio-analyzer | 포트폴리오 분석 |
| `/company-research` | company-job-researcher | 기업·직무(JD) 분석 |
| `/resume-build` | resume-lead→resume-writer | 기업 맞춤 이력서 |
| `/portfolio` | resume-lead→portfolio-writer | 기업 맞춤 포트폴리오 |
| `/resume-review` | resume-reviewer | 검수 리포트(게이트) |
| `/drive-sync` | docs-formatter | Docs 서식·Drive 저장 |

---

## 🔄 협업 흐름

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

---

## 🚧 다른 팀과의 경계 (핸드오프)

| 필요 | 핸드오프 |
|---|---|
| 자기소개서·블로그 등 **범용 마케팅 카피** | `marketing_agents` |
| 포트폴리오 **사이트/코드 구현·리팩터** | `dev_agents` |
| 포트폴리오 **비주얼·디자인 시스템** | `design_agents` |

> 우리는 *경력 근거 → 기업 맞춤 이력서·포트폴리오 문서*를 만듭니다. 코드·비주얼·범용 카피는 해당 팀으로 넘깁니다.

---

## 🔌 MCP / 권한 (요약)
- **Google Workspace**: `docs`·`drive` 도구만 활성화(권한 표면 최소화). 쓰기 가능 → 덮어쓰기 전 사용자 확인.
- **GitHub**: 공식 원격 MCP, **읽기 전용** 권장(경력 근거). fine-grained PAT를 `GITHUB_PAT` 환경변수로 주입(Contents/Metadata=Read-only). 이 엔드포인트는 자동 OAuth(DCR) 미지원 — PAT가 비면 `/mcp`에서 HTTP 400.
- 기업·직무 조사: 빌트인 **WebSearch/WebFetch**(별도 MCP 없음).
- **비밀값은 레포 금지** — `${ENV}` 치환 + `.gitignore`(client_secret·token·.env 류 차단).

---

## 📂 폴더 구조

```
resume_agents/
├── CLAUDE.md            # 그룹장(오케스트레이터)
├── README.md           # (이 파일) 빠른 시작
├── .gitignore
├── .mcp.json           # Google Workspace(docs/drive) + GitHub 원격 MCP
├── .claude/
│   ├── agents/         # 에이전트 8종 (자동 로드)
│   └── skills/         # 슬래시 스킬 8종
├── templates/          # 산출물 템플릿 5종
└── docs/               # 워크플로우 · 서식 · 검수 체크리스트 · MCP 가이드
```

---
🤖 Built with [Claude Code](https://claude.com/claude-code)
