# docs-agents 📝

> 우리 팀의 **AI 문서화 팀**. Claude Code로 기획·개발 문서를 표준 형식에 맞춰
> 작성하고, 검토받고, Google Drive에 정리합니다. **개발 지식이 없어도 쓸 수 있습니다.**

`garrytan/gstack` 스타일의 체계화된 에이전트 팀 구조를 문서 워크플로우에 맞게 구성했습니다.

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
| 기획서 | `/planning-doc` | 기획자 |
| 요구사항 정의서 | `/requirements-doc` | 요구사항 분석가 |
| 기능명세서 | `/functional-spec` | 명세 작성자 |
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
| (모든 문서) 검수·첨삭 | `/doc-review` | 검수자 |
| (모든 문서) Drive 반영·서식 | `/drive-sync` | 서식 전문가 |

> 빗금 명령을 몰라도 "회의록 만들어줘"처럼 **말로 시켜도** 됩니다.

> 📅 **일정도 다룹니다.** "이번 주 일정 정리해줘", "릴리즈 일정 등록해줘"처럼 시키면
> Google Calendar를 조회·생성·수정합니다(`/calendar`). 표형 문서는 Google **Sheets에 직접
> 작성·수정**합니다. *일정 삭제·대량 변경은 사람 확인을 거칩니다.*

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
└── docs/                    # 사용 설명서 · 워크플로우 · 스타일 · 연결 가이드
```

---

## 문서

- 🟢 [비개발자용 사용 설명서 (처음이라면)](docs/GETTING-STARTED.md)
- [Google Drive 연결 가이드](docs/GOOGLE-DRIVE-MCP.md)
- [작업 워크플로우](docs/WORKFLOW.md)
- [한국어 문서 스타일 가이드](docs/STYLE-GUIDE.md)
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
