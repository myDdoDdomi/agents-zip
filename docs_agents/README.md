# docs-agents 📝

> 우리 팀의 **AI 문서화 팀**. Claude Code로 기획·개발 문서를 표준 형식에 맞춰
> 작성하고, 검토받고, Google Drive에 정리합니다. **개발 지식이 없어도 쓸 수 있습니다.**

`garrytan/gstack`(Claude Code를 가상 엔지니어링 팀으로 만드는 스킬 툴킷)의 검증된 아이디어
3가지를 문서팀 맥락으로 이식했습니다: **① 요구 인터뷰**(`/office-hours` — 착수 전 포싱 질문으로
요구·갭·가정·성공기준 확정), **② Diátaxis 문서 분류**(튜토리얼/하우투/레퍼런스/설명 4분면으로
문서 성격을 먼저 정함 → `docs/DIATAXIS.md`), **③ 다관점 심화 검수**(`/deep-review` — 중요 문서를
5개 렌즈로 병렬 검수 후 1개 판정으로 종합).

> 🟢 **처음이세요? → [비개발자용 사용 설명서](docs/GETTING-STARTED.md) 부터 보세요.**

---

## 빠른 시작 (3단계)

1. **이 폴더를 받아** Claude Code로 엽니다.
2. **Google Drive를 1회 연결**합니다. → [연결 가이드](docs/GOOGLE-DRIVE-MCP.md)
3. 입력창에 **한국어로 시킵니다.**
   ```
   출석 체크 기능 기획서 만들어줘
   ```
   → AI가 초안 작성 → "검수해줘" → "고쳐줘" → "드라이브에 올려줘" 순으로 진행합니다.

> 빗금(`/`)을 치면 만들 수 있는 문서 목록이 떠요. 외울 필요 없이 골라 쓰면 됩니다.

---

## 문서 종류와 담당

| 문서 | 빗금 명령 | 담당 |
|---|---|---|
| (착수 전) 요구 인터뷰 | `/office-hours` | 인테이크 인터뷰어 |
| 레퍼런스 리서치 | `/reference-research` | 제품 리서처 |
| 인사이트 분석 | `/insight-analysis` | 인사이트 분석가 |
| 기획서 | `/planning-doc` | 기획자 |
| 요구사항 정의서 | `/requirements-doc` | 요구사항 분석가 |
| 기능명세서 | `/functional-spec` | 명세 작성자 |
| 기술명세서 | `/tech-spec` | 명세 작성자 |
| 정보구조도(IA) | `/ia` | UX 설계자 |
| 메뉴트리 | `/menu-tree` | UX 설계자 |
| 유저 플로우차트 | `/user-flow` | UX 설계자 |
| 와이어프레임 | `/wireframe` | UX 설계자 |
| 화면정의서 | `/screen-spec` | 명세 작성자 |
| API 명세서 | `/api-spec` | API 설계자 |
| DB 설계서(ERD) | `/db-design` | 데이터 모델러 |
| 시퀀스 다이어그램 | `/sequence-diagram` | 다이어그램 작성자 |
| 테스트케이스 | `/test-case` | QA 엔지니어 |
| 회의록 | `/meeting-notes` | 테크니컬 라이터 |
| 릴리즈노트 | `/release-notes` | 테크니컬 라이터 |
| 일정(캘린더) | `/calendar` | 일정 관리자 |
| 일정계획(간트·칸반) | `/project-plan` | 프로젝트 일정 관리자(PM) |
| (모든 문서) 검수·첨삭 | `/doc-review` | 검수자 |
| (중요 문서) 다관점 심화 검수 | `/deep-review` | 검수자(+다관점 렌즈) |
| (모든 문서) Drive 반영·서식 | `/drive-sync` | 서식 전문가 |
| (회고) 팀 작업 피드백 보고 | `/feedback-agents` | 피드백 리포터 |

> 빗금 명령을 몰라도 "회의록 만들어줘"처럼 **말로 시켜도** 됩니다.

> 🔎 **기획 전, 리서치부터 합니다.** "경쟁 제품 레퍼런스 조사해줘", "벤치마크해줘"처럼 시키면
> 외부 사례·기능/UX 벤치마크·아이디어를 **출처와 함께** 모으고(`/reference-research`), 이어서
> "인사이트 뽑아줘"로 패턴·기회·적용 제안을 도출합니다(`/insight-analysis`). 흐름은
> **레퍼런스 리서치 → 인사이트 분석 → 기획서**입니다. *(시장규모·GTM 같은 마케팅 리서치는 marketing 팀 몫입니다.)*

> 📅 **일정도 다룹니다.** "이번 주 일정 정리해줘", "릴리즈 일정 등록해줘"처럼 시키면
> Google Calendar를 조회·생성·수정합니다(`/calendar`). 표형 문서는 Google **Sheets에 직접
> 작성·수정**합니다. *일정 삭제·대량 변경은 사람 확인을 거칩니다.*

> 📊 **일정 계획도 만듭니다.** "간트차트 만들어줘", "칸반보드", "일정 계획 짜줘"처럼 시키면
> 작업을 분해(WBS)하고 **간트차트 + 칸반보드를 한 개의 스프레드시트**로 만듭니다(`/project-plan`).
> `Tasks` 시트에서 담당자·상태만 바꾸면 간트·칸반이 따라 갱신됩니다(단방향). *추측 일정은 만들지 않습니다.*

---

## 표준 워크플로우

```
① 초안 → ② 검수·첨삭 → ③ 수정 → ④ Drive 반영
```
자세한 흐름: [docs/WORKFLOW.md](docs/WORKFLOW.md)

---

## 폴더 구조

```
docs-agents/
├── CLAUDE.md                # 그룹장(팀 운영 기준)
├── .mcp.json                # 팀 공유 Google Drive 연결 설정 (비밀값 없음)
├── config/drive-config.md   # 공유 Drive 폴더 매핑 (팀 리드가 1회 채움)
├── .claude/
│   ├── agents/              # 에이전트 팀원(담당 AI)
│   └── skills/              # 문서종류별 빗금(/) 명령
├── templates/               # 한국어 문서 템플릿
├── examples/                # 범용 서식판 예시 (다른 팀 참고용)
├── feedback/                # 팀 작업 회고·피드백 보고서 (→ 본부) /feedback-agents
└── docs/                    # 사용 설명서 · 워크플로우 · 스타일 · 연결 가이드
```

---

## 문서

- 🟢 [비개발자용 사용 설명서 (처음이라면)](docs/GETTING-STARTED.md)
- [Google Drive 연결 가이드](docs/GOOGLE-DRIVE-MCP.md)
- [작업 워크플로우](docs/WORKFLOW.md)
- [한국어 문서 스타일 가이드](docs/STYLE-GUIDE.md)
- [Diátaxis 문서 분류 프레임워크](docs/DIATAXIS.md)
- [Drive 서식 표준](docs/DOC-FORMATTING.md)

---

## ⚠️ 보안 (공유 전 필독)

- **개인 인증 키는 절대 레포에 올리지 마세요.** `client_secret*.json`, `.env`,
  `.claude/settings.local.json` 등은 `.gitignore`로 자동 제외됩니다.
- 새 팀원은 **본인 Google 계정으로 직접 OAuth 키를 발급**합니다. (→ 연결 가이드)
- **zip으로 폴더를 통째 공유하지 마세요.** git clone/pull로 받으면 비밀 파일이 자동 제외됩니다.
- `config/drive-config.md`에 실제 폴더 ID를 채웠다면, 외부 공유 전 확인하세요.

---

## 팀에 기여하기 (새 문서 종류/에이전트 추가)

1. `templates/`에 템플릿 추가
2. `.claude/agents/`에 담당 에이전트 추가 (또는 기존 에이전트에 매핑)
3. `.claude/skills/<이름>/SKILL.md`에 빗금 명령 추가
4. `CLAUDE.md` §4 라우팅 표에 한 줄 추가
5. PR 생성

---
🤖 Built with [Claude Code](https://claude.com/claude-code)

## 🧠 팀 메모리 (MEMORY.md) 사용법

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
