# dev-agents — 팀 메모리 (MEMORY.md)

> **이 파일은 팀의 "학습 누적층"이다.** 그룹장(CLAUDE.md)이 `@./MEMORY.md`로 매 세션 자동 로드한다.
> CLAUDE.md(사람이 쓰는 안정적 규칙·라우팅)와 역할이 다르다 — 여기엔 **작업하며 검증된 함정·우회법·환경 매핑·반복 교정**처럼 *바뀌고 쌓이는 운영 지식*을 적는다.
>
> **유지 규칙:** ① 200줄 이하 유지(초과분은 `memory/<주제>.md`로 분리—온디맨드). ② 한 항목=한 사실(가능하면 날짜·근거). ③ CLAUDE.md와 모순 금지. ④ 틀린 학습은 삭제. ⑤ 비밀값 금지.
> **언제 적나:** 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · `/feedback-agents` 회고에서 즉시 재사용 가능한 학습.

## 1. 검증된 함정·우회법 (Gotchas & Workarounds)
- (아직 누적된 학습 없음 — 작업하며 채움)

## 2. 환경·프로젝트 매핑 (Env & Project Mapping)
- MCP: **GitHub + Context7** 사용. **모델 B(복사-라이브러리):** `agents/*.md`만 대상 프로젝트 `.claude/agents/`로 복사(CLAUDE.md·MEMORY.md는 비복사, HQ 측 기관기억).

## 3. 반복 교정·선호 (Recurring Corrections / Preferences) — 레드팀 안전 수칙
- **레드팀은 인가·ROE 게이트가 먼저다**(`redteam-lead §0`). 범위·대상·시간·연락선을 사용자에게 확인하기 전엔 공격 시작 금지.
- **비파괴가 기본.** 실제 익스플로잇·데이터 추출·파괴적/DoS 행위는 **사용자/그룹장 명시 승인 후에만**. 측면이동(lateral movement) 금지.
- **대상은 사용자 소유/명시 인가 자산만.** 제3자·공유 클라우드 인프라·범위 밖은 거부.

## 4. 도구·명령 메모 (Tools & Commands)
- `/pentest`(레드팀 공격) → 리포트 → `/remediate`(블루팀 방어) **퍼플팀 루프**.

## 5. 미해결·주의 (Open Issues / Cautions)
- **비밀값·발견사항(취약점 상세)은 레포에 커밋 금지.** `pentest-reports/`·`security/`는 `.gitignore`에 포함됨.
