# resume-agents — 팀 메모리 (MEMORY.md)

> **이 파일은 팀의 "학습 누적층"이다.** 그룹장(CLAUDE.md)이 `@./MEMORY.md`로 매 세션 자동 로드한다.
> CLAUDE.md(사람이 쓰는 안정적 규칙·라우팅)와 역할이 다르다 — 여기엔 **작업하며 검증된 함정·우회법·환경 매핑·반복 교정**처럼 *바뀌고 쌓이는 운영 지식*을 적는다.
>
> **유지 규칙:** ① 200줄 이하 유지(초과분은 `memory/<주제>.md`로 분리—온디맨드). ② 한 항목=한 사실(가능하면 날짜·근거). ③ CLAUDE.md와 모순 금지. ④ 틀린 학습은 삭제. ⑤ 비밀값 금지.
> **언제 적나:** 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · `/feedback-agents` 회고에서 즉시 재사용 가능한 학습.

## 1. 검증된 함정·우회법 (Gotchas & Workarounds)
- [2026-06] GitHub 원격 MCP 인증은 **OAuth가 아니라 PAT(Personal Access Token)** 를 쓴다(OAuth 시도 시 HTTP 400 발생).
- Google Workspace import 함정은 docs팀과 동일: `import_to_google_sheets`는 **`C:\Users\info\.workspace-mcp\attachments\` 내부 + `file://`** 만 허용, `file_name`은 마지막 점 뒤를 확장자로 strip(점 없는 임시명 import 후 리네임).

## 2. 환경·프로젝트 매핑 (Env & Project Mapping)
- (아직 누적된 학습 없음 — 작업하며 채움: 지원기업 JD·GitHub 핸들·산출 Drive 폴더 매핑)

## 3. 반복 교정·선호 (Recurring Corrections / Preferences)
- [2026-06] 이력서·포트폴리오 **양식(서식)은 빌드→import 파이프라인**으로 산출(양식 안정화 완료). 매 작업 재서식보다 양식 재사용을 우선.

## 4. 도구·명령 메모 (Tools & Commands)
- 스킬 이름 충돌 해소: 이력서 **빌드는 `/resume-build`** (예약/충돌 회피로 `/resume`에서 변경).

## 5. 미해결·주의 (Open Issues / Cautions)
- (아직 누적된 학습 없음 — 작업하며 채움)
