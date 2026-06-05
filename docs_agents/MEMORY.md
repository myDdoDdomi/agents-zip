# docs-agents — 팀 메모리 (MEMORY.md)

> **이 파일은 팀의 "학습 누적층"이다.** 그룹장(CLAUDE.md)이 `@./MEMORY.md`로 매 세션 자동 로드한다.
> CLAUDE.md(사람이 쓰는 안정적 규칙·라우팅)와 역할이 다르다 — 여기엔 **작업하며 검증된 함정·우회법·환경 매핑·반복 교정**처럼 *바뀌고 쌓이는 운영 지식*을 적는다.
>
> **유지 규칙:** ① 200줄 이하 유지(초과분은 `memory/<주제>.md`로 분리—온디맨드). ② 한 항목=한 사실(가능하면 날짜·근거). ③ CLAUDE.md와 모순 금지. ④ 틀린 학습은 삭제. ⑤ 비밀값 금지.
> **언제 적나:** 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · `/feedback-agents` 회고에서 즉시 재사용 가능한 학습.

## 1. 검증된 함정·우회법 (Gotchas & Workarounds)
- [2026-06-03] Google Sheets는 이 MCP에 **셀 서식 직접 API가 없다**(배경색·틀고정·드롭다운·조건부서식 불가). → `openpyxl`로 `.xlsx`를 빌드해 `import_to_google_sheets`로 업로드하면 서식·다중탭·드롭다운·조건부서식이 **보존**된다(실측). 상세: `docs/DOC-FORMATTING.md §3-A`.
- [2026-06-03] `import_to_google_sheets`의 `file_path`는 **`C:\Users\info\.workspace-mcp\attachments\` 내부 파일 + `file://` URL만** 허용(절대 Windows 경로 거부). 상세: `docs/GOOGLE-DRIVE-MCP.md §C-1`.
- [2026-06-03] import 시 `file_name`은 **마지막 점 뒤를 확장자로 잘라낸다**(`…_v3.1`→`v3`). → 점 없는 임시명으로 import 후 `update_drive_file(name=…)`로 리네임.
- [2026-06-03] 공유 Drive 파일 휴지통 처리(`update_drive_file(trashed=true)`)는 자동 분류기에 **1회 차단**될 수 있다 → 그룹장이 사용자 확인 후 일괄 정리(Human-in-the-loop).

## 2. 환경·프로젝트 매핑 (Env & Project Mapping)
- 프로젝트 ↔ Drive 폴더 매핑은 `config/drive-config.md`가 정본. (예: **동행 AI = `Team동행` 폴더**)

## 3. 반복 교정·선호 (Recurring Corrections / Preferences)
- [2026-06-03] 표 중심 문서는 **착수 전 매체(Sheets/Docs)를 1줄로 확정**한다(매체 왕복이 최대 낭비원).
- [2026-06-03] **상위 문서 매체 승계**: 요구사항정의서가 Sheets면 기능명세·기술명세·테스트케이스도 Sheets가 기본.

## 4. 도구·명령 메모 (Tools & Commands)
- 큰 콘텐츠(예: 34행 명세)는 **Drive/로컬에 먼저 저장**하고 후속 에이전트는 `get_drive_file_content`/`read_sheet_values`로 **읽게** 한다(인라인 재전송 금지).
- 동일 작업 연속은 새 spawn 대신 **SendMessage 이어쓰기**로 컨텍스트 재사용.

## 5. 미해결·주의 (Open Issues / Cautions)
- (아직 누적된 학습 없음 — 작업하며 채움)
