# team-agents 본부 — 메모리 (MEMORY.md)

> **이 MEMORY.md는 *본부(team-architect)* 의 자기개선 학습 누적층이다.** 어떤 IT 트렌드가 어느 팀에
> 어떻게 반영됐는지 · 반복되는 선별/적용 판단 기준 · 팀 빌딩에서 검증된 함정·환경 매핑을 누적한다.
> `/update-agents` 사이클의 *즉시 재사용 가능한* 학습을 여기에 적는다(본부 CLAUDE.md가 `@./MEMORY.md`로 매 세션 자동 로드).
>
> CLAUDE.md(사람이 쓰는 안정적 규칙·라우팅)와 역할이 다르다 — 여기엔 **작업하며 검증된 함정·우회법·환경 매핑·반복 교정**처럼 *바뀌고 쌓이는 운영 지식*을 적는다.
>
> **유지 규칙:** ① 200줄 이하 유지(초과분은 `memory/<주제>.md`로 분리—온디맨드). ② 한 항목=한 사실(가능하면 날짜·근거). ③ CLAUDE.md와 모순 금지. ④ 틀린 학습은 삭제. ⑤ 비밀값 금지.
> **언제 적나:** 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · `/update-agents`·`/feedback-agents` 회고에서 즉시 재사용 가능한 학습.

## 1. 검증된 함정·우회법 (Gotchas & Workarounds)
- (아직 누적된 학습 없음 — 작업하며 채움)

## 2. 환경·프로젝트 매핑 (Env & Project Mapping)
- 본부 전담팀(부서팀 아님): `trend-researcher`·`update-curator`·`team-fit-reviewer` + `/update-agents` 자기개선 파이프라인.

## 3. 반복 교정·선호 (Recurring Corrections / Preferences)
- [/update-agents 선별 기준] 경쟁 LLM(GPT/Gemini/Grok)·외부 오케스트레이터(LangGraph/CrewAI/OpenAI Agents SDK)는 §3-2 "공식 스택(Claude) 고정"으로 큐레이션에서 **탈락**시킨다(시장 지형 참고만). 시장 통계·채택률 수치도 실행 항목 아님 → 탈락. (2026-06-07)
- [/update-agents 적용 원칙] RC·초기 채택 단계(예: TS7 GA 전·Vue Vapor·Angular Zoneless·MCP RC)는 **조건부로만** 박는다("해당 스택/GA 확인 시"). 즉시 상시 발동 금지 — 과잉 적용 방지. (2026-06-07)
- [2026-06-07 1차 사이클 반영] AI/에이전트 보안(프롬프트 인젝션 LLM01·MCP 도구 포이즈닝·공급망)=dev·QA·본부 / 디자인 토큰(Figma↔Tailwind 1:1·다크-라이트 쌍)=design·QA / GEO·하이브리드 콘텐츠=marketing / 인간 편집 레이어 검수=marketing·docs·webnovel 에 반영함.

## 4. 도구·명령 메모 (Tools & Commands)
- (아직 누적된 학습 없음 — 작업하며 채움)

## 5. 미해결·주의 (Open Issues / Cautions)
- [워치리스트] MCP 2026-07-28 RC(stateless·MCP Apps UI·Tasks·JSON Schema 2020-12) 정식 릴리스 후 `.mcp.json`/MCP 표준 갱신 필요 — 릴리스 확인 시 `/update-agents` diff로 재점검 (2026-06-07, 근거: 동향 리서치).
