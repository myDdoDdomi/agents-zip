# design-agents — 팀 메모리 (MEMORY.md)

> **이 파일은 팀의 "학습 누적층"이다.** 그룹장(CLAUDE.md)이 `@./MEMORY.md`로 매 세션 자동 로드한다.
> CLAUDE.md(사람이 쓰는 안정적 규칙·라우팅)와 역할이 다르다 — 여기엔 **작업하며 검증된 함정·우회법·환경 매핑·반복 교정**처럼 *바뀌고 쌓이는 운영 지식*을 적는다.
>
> **유지 규칙:** ① 200줄 이하 유지(초과분은 `memory/<주제>.md`로 분리—온디맨드). ② 한 항목=한 사실(가능하면 날짜·근거). ③ CLAUDE.md와 모순 금지. ④ 틀린 학습은 삭제. ⑤ 비밀값 금지.
> **언제 적나:** 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · `/feedback-agents` 회고에서 즉시 재사용 가능한 학습.

## 1. 검증된 함정·우회법 (Gotchas & Workarounds)
- [2026-06-11 조립 회고] **`.mcp.json`은 에이전트가 생성·수정하지 못할 수 있다**(Claude Code 권한 게이트 차단) — figma 서버 블록 추가는 완성 JSON 제시 + 사용자 직접 붙여넣기(또는 사용자가 `claude mcp add` 실행). (`docs/FIGMA-MCP.md` §1)
- [2026-06-11 조립 회고] **조립 프로젝트 안의 유사 명칭 폴더는 HQ가 아니다** — 예: 어떤 프로젝트의 `dev\design_agents`는 그 프로젝트 전용 구버전. 재조립(/team-assemble) 원본은 항상 HQ 레포(`team-agents/design_agents`)에서 가져온다(구버전 오조립 = 기능·정책 불일치).

## 2. 환경·프로젝트 매핑 (Env & Project Mapping)
- MCP: **Figma 공식 MCP**로 디자인 추출·디자인-코드 정합 확인.
- `design-researcher`는 **21st.dev·shadcn 등 구현/컴포넌트 레지스트리·라이브 제품(Mobbin) 서칭** 가능(WebSearch/WebFetch). 실제 구현 레퍼런스는 **라이선스 확인 후 '개발 핸드오프 노트'로 `dev_agents`에 직행**(목업 스킵 가능). 멀티팀은 `/team-assemble`로 design+dev 조립. (2026-06-06, 근거: 사용자 지시)

## 3. 반복 교정·선호 (Recurring Corrections / Preferences)
- [2026-06-10 피드백] 멀티팀 조립 환경에서 dev가 디자인 토큰을 쓰는 FE 컴포넌트를 design 검토 없이 구현·프로덕션 배포 — design 팀은 핸드오프 요청을 기다리지 말고 **능동 제안**해야 게이트가 돈다(design-lead 책임 5·code-connect-mapper 협업 트리거 반영: 토큰 정합 + aria·색 단독 전달 접근성 설계 검토).

## 4. 도구·명령 메모 (Tools & Commands)
- (아직 누적된 학습 없음 — 작업하며 채움)

## 5. 미해결·주의 (Open Issues / Cautions)
- [워치리스트] Figma **Check Designs**가 공식 MCP 도구 호출로 실행 가능한지(API 공개 여부) 확인 필요 — UI 전용이면 `figma-operator` 가이드에 수동 실행→결과 텍스트 폴백 명시. (2026-06-10, 근거: /update-agents)
- [Figma Agent] 베타(2026-05-20~ 롤아웃, Full seat 전용, 베타 중 크레딧 무료). 외부 에이전트 연동 MCP 도구 `use_figma` 공개 — MCP 호출 가능 시 조건부 활용 검토. (2026-06-10 재실행 검증)
