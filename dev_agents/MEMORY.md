# dev-agents — 팀 메모리 (MEMORY.md)

> **이 파일은 팀의 "학습 누적층"이다.** 그룹장(CLAUDE.md)이 `@./MEMORY.md`로 매 세션 자동 로드한다.
> CLAUDE.md(사람이 쓰는 안정적 규칙·라우팅)와 역할이 다르다 — 여기엔 **작업하며 검증된 함정·우회법·환경 매핑·반복 교정**처럼 *바뀌고 쌓이는 운영 지식*을 적는다.
>
> **유지 규칙:** ① 200줄 이하 유지(초과분은 `memory/<주제>.md`로 분리—온디맨드). ② 한 항목=한 사실(가능하면 날짜·근거). ③ CLAUDE.md와 모순 금지. ④ 틀린 학습은 삭제. ⑤ 비밀값 금지.
> **언제 적나:** 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · `/feedback-agents` 회고에서 즉시 재사용 가능한 학습.

## 1. 검증된 함정·우회법 (Gotchas & Workarounds)
- **코어 경로 silent catch는 치명적 실패를 영구 은폐**(2026-06-07, Chesslytics): best-effort `except`가 코어 데이터 생성 경로에 있으면 422 등 실패가 매번 삼켜져 파이프라인 전체 비동작을 여러 사이클 은폐. 코어 경로엔 error 로깅·관측성·상태 신호 필수(보조 경로만 best-effort 허용). → code-reviewer/dev-lead 본문에 반영됨.
- **mock-prod 패리티 미점검이 최대 반복 결함 원인**(2026-06-07, FitMate): mock에만 있고 실 구현체에 스텁(`return []`/`null`/not-implemented)으로 남은 메서드가 prod에서만 깨짐 — 정적 검사·단위테스트 통과. types 인터페이스 대비 실구현 완성도 self-check. → implementer/code-reviewer 본문에 반영됨.
- **Firebase Functions v2 시크릿 충돌**(2026-06-07, FitMate): `defineSecret`으로 선언한 시크릿을 `functions/.env`에 동일 이름으로 두면 충돌해 배포 실패. → `.secret.local`(에뮬레이터) 또는 Secret Manager(배포)에 둔다.
- **에뮬레이터 장기 구동 degradation**(2026-06-07, FitMate): 수 시간·수십 회 핫리로드 후 함수 워커가 `deadline-exceeded` 반환 — 코드 회귀로 오판하기 쉬움. 회귀 결론 전에 **에뮬레이터 재기동 먼저** 확인(코드 무변경으로 해소됨).
- **서버리스 런타임 import 해석 차이**(2026-06-07, FitMate): `admin.firestore.FieldValue`가 에뮬레이터 런타임에서 `undefined`로 해석돼 정적분석·단위테스트를 통과하고 E2E에서야 발견. 모듈러 직접 import(`require("firebase-admin/firestore")`)로 해소. → 백엔드 핵심 경로 변경은 통합 스모크 필수(implementer 본문 반영).

## 2. 환경·프로젝트 매핑 (Env & Project Mapping)
- MCP: **GitHub + Context7** 사용. **모델 B(복사-라이브러리):** `agents/*.md`만 대상 프로젝트 `.claude/agents/`로 복사(CLAUDE.md·MEMORY.md는 비복사, HQ 측 기관기억).

## 3. 반복 교정·선호 (Recurring Corrections / Preferences) — 레드팀 안전 수칙
- **레드팀은 인가·ROE 게이트가 먼저다**(`redteam-lead §0`). 범위·대상·시간·연락선을 사용자에게 확인하기 전엔 공격 시작 금지.
- **비파괴가 기본.** 실제 익스플로잇·데이터 추출·파괴적/DoS 행위는 **사용자/그룹장 명시 승인 후에만**. 측면이동(lateral movement) 금지.
- **대상은 사용자 소유/명시 인가 자산만.** 제3자·공유 클라우드 인프라·범위 밖은 거부.

## 4. 도구·명령 메모 (Tools & Commands)
- `/pentest`(레드팀 공격) → 리포트 → `/remediate`(블루팀 방어) **퍼플팀 루프**.
- **Cloud Run env 플래그**(2026-06-07, Chesslytics): `--set-env-vars`=치환(이전 env 전체 소멸), `--update-env-vars`=병합(기존 유지+추가). 수동 주입 비밀값이 있는 서비스는 재배포 전 `gcloud run services describe`로 현재 env 확인 후 병합 플래그 사용.
- **gcloud 전역 config 오염**(2026-06-07): 개발 머신 전역 config가 타 프로젝트를 가리킬 수 있음 → 배포 명령에 `--project <id> --account <email>` 명시, 배포 전 `gcloud config list` 확인.
- **Cloud Run 장기 잡 기본값**(2026-06-07, Chesslytics): 요청 타임아웃 기본 60s → 긴 잡은 `--timeout=300`~ 상향. 응답 후 CPU throttling으로 백그라운드 잡 멈춤 → 요청 내 완주 필요 시 `--no-cpu-throttling`. HTTP 클라 타임아웃을 런타임 타임아웃 이하로 정합.
- **Firebase Hosting 캐시 기본값**(2026-06-07, FitMate): `index.html` 캐시 미설정 시 브라우저 1시간 캐시 → 배포 후 "옛 화면" 혼란. SPA는 `firebase.json headers`로 진입 HTML `Cache-Control: no-cache`, 해시 자산(`/assets/**`) `public, max-age=31536000, immutable`.

## 5. 미해결·주의 (Open Issues / Cautions)
- **비밀값·발견사항(취약점 상세)은 레포에 커밋 금지.** `pentest-reports/`·`security/`는 `.gitignore`에 포함됨.
- **유료 생성 API billing 한도는 계정 레벨일 수 있음**(2026-06-07, FitMate): OpenAI billing hard limit은 API 키가 아닌 **계정 월 한도** → 키를 바꿔도 해소 안 됨. 해소는 계정 콘솔 한도 상향 또는 다음 달 리셋. 유료 생성 API는 사양 확정 후 호출(implementer 게이트).
