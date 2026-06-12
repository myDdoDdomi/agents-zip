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
- [멀티팀 재배포 사고] 수동 복사로 팀 업데이트를 재배포하면 오배치(docs 16종이 `agents/dev/`로)·협업 CLAUDE.md 통째 덮어쓰기(오케스트레이터 블록·타 팀 본문 소실) 사고가 구조적으로 발생(2026-06-10, 조립 프로젝트 회고) → `/team-assemble` §1-2 재배포 체크리스트 신설(팀별 폴더 확인·CLAUDE.md 덮어쓰기 금지·diff 후 교체+백업·재실행 검증).
- [Miasma 공급망] npm OIDC trusted publishing 악용+SLSA 증명 위조로 신뢰체인 자체가 공격면(2026-06-01). 외부 MCP/도구가 패키지 레지스트리 의존성을 가지면 preinstall/postinstall 훅 실행 여부 점검·OIDC 신뢰 가정 금지·provenance 독립 검증. read-only·전용 계정 우선 원칙 유지.
- [.mcp.json 편집 게이트] 에이전트는 `.mcp.json`(MCP 설정 파일)을 생성·수정하지 못할 수 있다 — Claude Code 권한 게이트가 차단(2026-06-11 조립 프로젝트, 2회 확인). `/team-assemble` §2-1b 자동 포트의 `.mcp.json` 병합도 차단될 수 있음 → 우회 금지, 완성 JSON 블록 제시 + 사용자 직접 붙여넣기로 폴백(pm·design MCP 가이드에 경고 반영).
- [재조립 원본 혼동] 조립 프로젝트 안의 유사 명칭 팀 폴더(예: `dev\design_agents` 구버전)를 HQ로 오인해 재조립하면 기능·정책 불일치 — 복사 원본은 항상 HQ 레포(`/team-assemble` §1-2에 반영, 2026-06-11).
- [Atlassian MCP 엔드포인트] 구 SSE(`/v1/sse`, type: sse)는 2026-06-30 이후 지원 종료(공식 문서, 2026-06-11 검증) → pm_agents `.mcp.json`·가이드를 `type: http` + `https://mcp.atlassian.com/v1/mcp`로 이행함. 피드백 보고서의 버전·URL류 주장도 1차 출처 재확인 후 적용(§3 검증 학습과 동일 원칙).

## 2. 환경·프로젝트 매핑 (Env & Project Mapping)
- 본부 전담팀(부서팀 아님): `trend-researcher`·`update-curator`·`team-fit-reviewer` + `/update-agents` 자기개선 파이프라인.

## 3. 반복 교정·선호 (Recurring Corrections / Preferences)
- [/update-agents 선별 기준] 경쟁 LLM(GPT/Gemini/Grok)·외부 오케스트레이터(LangGraph/CrewAI/OpenAI Agents SDK)는 §3-2 "공식 스택(Claude) 고정"으로 큐레이션에서 **탈락**시킨다(시장 지형 참고만). 시장 통계·채택률 수치도 실행 항목 아님 → 탈락. (2026-06-07)
- [/update-agents 적용 원칙] RC·초기 채택 단계(예: TS7 GA 전·Vue Vapor·Angular Zoneless·MCP RC)는 **조건부로만** 박는다("해당 스택/GA 확인 시"). 즉시 상시 발동 금지 — 과잉 적용 방지. (2026-06-07)
- [2026-06-07 1차 사이클 반영] AI/에이전트 보안(프롬프트 인젝션 LLM01·MCP 도구 포이즈닝·공급망)=dev·QA·본부 / 디자인 토큰(Figma↔Tailwind 1:1·다크-라이트 쌍)=design·QA / GEO·하이브리드 콘텐츠=marketing / 인간 편집 레이어 검수=marketing·docs·webnovel 에 반영함.
- [사실 교정] 'Node.js 25 EOL' 표현은 오류 — Node 25는 없음(짝수 LTS 체계). v26이 Current(v26.3.0, Temporal 기본 활성, 2026-10 LTS 예정). (2026-06-10)
- [/update-agents 재실행 검증 학습] 사이클 산출의 **수치·날짜·버전 단계**(가격, RC/베타/GA, 발효일)는 오류 빈발 영역 — 적용 전 1차 출처 재확인. 2026-06-10 재실행에서 오전(Opus 4.8) 사이클 오류 6건 교정(EU 발효·MCP RC·Node·TS6·Fable 가격·Figma 시점). 당일 사이클의 실행 모델은 세션 transcript(`~/.claude/projects/<프로젝트>/*.jsonl`)의 `"model"` 필드 집계로 검증 가능.

- [피드백 반영 패턴 2026-06-10] 경계 결함은 "차원을 바꿔" 재발한다(계약 드리프트 casing→필드→부호→시점 4세대, 조립 프로젝트) → 점검 항목은 단건 추가가 아니라 **차원 목록(체크리스트)으로 구조화**해 반영. 핸드오프 생략 같은 프로세스 결함은 한쪽 의무화로 부족 — **양 팀 이중 트리거**(dev DoD 필수화 + qa-lead 능동 제안)로 박는다.
- [피드백 반영 패턴 2026-06-11] 환경 제약(현지어 이슈타입·응답 토큰 한도·무료 플랜 한계·편집 게이트)은 버그가 아니라 **운영 전제**다 — 에이전트 정의·가이드에 폴백·확인 절차를 자체 내장해야 같은 장벽을 매번 다시 만나지 않는다. 반영처: pm(JQL 현지어·대량조회·Slack 폴백·WRITE-GATE 표 선행) / pm·design·본부(.mcp.json 편집 게이트) / 조립(시크릿 스캔 선행·HQ 원본 고정).

## 4. 도구·명령 메모 (Tools & Commands)
- [Claude Code 6-08 패치] --safe-mode(전체 커스터마이징 비활성 — CLAUDE.md/Skills 디버깅용)·/cd(작업 디렉터리 변경, 캐시 유지)·disableBundledSkills 설정. [6-10 패치] fallbackModel 최대 3단계·Hooks additionalContext 반환·PowerShell 행업 수정(win32 본 레포 직접 유용). (2026-06-10)

## 5. 미해결·주의 (Open Issues / Cautions)
- [워치리스트] MCP RC는 2026-05-21 이미 공개(stateless 코어·Tasks·MCP Apps·Extensions·SEP 6개), 정식 릴리스 2026-07-28 예정 — 정식 릴리스 후 `.mcp.json`/MCP 표준 갱신, 릴리스 확인 시 `/update-agents` diff로 재점검 (2026-06-10, 재실행 검증).
- [크레딧] Claude Code 청구 구조 변경 2026-06-15 시행 — Agent SDK·`claude -p`·GitHub Actions·서드파티 에이전트가 구독 한도와 분리(별도 월 크레딧, 롤오버 없음, 소진 시 요청 중단). 팬아웃·`/tech-briefing` 등 대량 병렬 실행 전 잔여 크레딧 확인 권장. 수치는 정책 변동 가능 → 하드코딩 말 것.
- [모델] Claude Fable 5·Mythos 5 GA(2026-06-09) — 별칭 매핑은 변경 없음(별도 라인). 전 팀 에이전트 model: 재배정은 보류(보수적 기본값). 신모델 상시 채택 필요성 생기면 운영노트 라인업 기준 재검토.
