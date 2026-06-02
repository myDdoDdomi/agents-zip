# resume-agents 팀 사용 피드백

- **작성일:** 2026-06-02
- **사용자:** 최봉준 (지원자 본인)
- **사용 시나리오:** ㈜이카운트 서버개발(신입) 지원서 작성 — GitHub(myDdoDdomi)·포트폴리오(bong-pf-pjt.web.app) 근거 수집 → 기업 맞춤 작성 → 검수 → Google Drive 반영, 이어서 GitHub 프로필 README 개편
- **사용 에이전트/스킬:** github-career-extractor, portfolio-analyzer, resume-writer, resume-reviewer, docs-formatter + Google Workspace MCP / GitHub MCP

---

## 1. 한 줄 총평

근거 기반 거버넌스(추측·과장 금지)와 검수 게이트는 **기대 이상으로 잘 작동**했다. 다만 **Google Workspace MCP 인증 셋업**과 **공식 양식(.docx) 자동 채우기**에서 마찰이 커서, 이 두 곳의 표준 절차를 보강하면 체감 완성도가 크게 오를 것.

---

## 2. 좋았던 점 (Keep)

1. **근거 기반 원칙이 실제로 지켜짐.** GitHub/포트폴리오에서 추출한 사실만으로 작성하고, 불확실한 항목은 `(확인 필요)`로 막아줬다. "팀 레포 미러라 본인 기여를 단정 못 한다"까지 잡아낸 점이 신뢰감 있었다.
2. **검수 게이트(resume-reviewer)가 진짜로 걸러냈다.** "수십만 기업" 같은 미확인 수치, PushOfLife의 단독 주도 표현, 팀 프로젝트 세부 단정 등을 🔴로 잡고 수정 전 Drive 반영을 막았다. 특히 **요약만으로는 통과를 안 주고 "실제 본문을 봐야 판정 가능"**이라고 거부한 건 게이트의 존재 이유에 충실했다.
3. **근거 수집 에이전트가 유용.** github-career-extractor가 레포·스택·기여를, portfolio-analyzer가 프로젝트 역할(팀장·BE 등)을 출처와 함께 정리해줘 작성 기반이 탄탄했다.
4. **기업 적합도 정렬.** 이카운트=Java/Spring·RDB·정합성 ERP라는 기준으로 프로젝트 순서·강조를 잡아준 게 도움이 됐다.
5. **사실 보강 루프.** 프로필과 이력서의 어학·수상 불일치(JLPT N2/OPIc IM2/SSAFY 수상)를 발견해 되물어, 누락된 실제 강점을 찾아낸 점이 좋았다.

---

## 3. 아쉬웠던 점 / 개선 필요 (Problem)

1. **Google Workspace MCP 인증 진입장벽이 높음.**
   - OAuth 클라이언트 시크릿 경로/계정 불일치, redirect_uri(`localhost:8000/oauth2callback`) 누락, 테스트 앱 Test users 미등록(access_denied) 등으로 여러 번 막혔다.
   - 파일 입력 도구(import)가 `~/.workspace-mcp/attachments` 밖 경로를 못 읽는 제약도 처음엔 당황스러웠다.

2. **공식 양식(.docx) 자동 채우기가 양식을 깨뜨림 — 가장 큰 마찰.**
   - 변환된 Google Doc의 본문이 **탭(t.0) 안**에 있어 docs-formatter가 본문을 비어있다고 오판.
   - 셀 병합이 많은 양식에서 셀 인덱스 삽입이 **엉뚱한(라벨) 셀로 들어가 표가 깨짐.** 결국 마크다운 import로 **새 Doc 생성**이 안정적이었다.

3. **docs-formatter의 멈춤/재질문.**
   - 이미 검수 통과한 산출물인데 "검수 통과했냐"를 반복 질문하며 멈췄다. 상태(검수 결과) 전달 프로토콜이 약하다.

4. **에이전트 이어받기(SendMessage) 불가.**
   - 한 번 띄운 작성/포맷 에이전트를 이어서 못 부르고 매번 새로 컨텍스트를 재전달해야 해 토큰·시간 낭비가 있었다.

5. **부분 편집 수단 부재.**
   - 한 항목만 바꿔도 Drive 문서 전체 내용을 다시 전송(update)해야 했다. 표 한 칸 수정에도 전체 재변환.

---

## 4. 개선 제안 (Try)

1. **docs-formatter 규칙 보강:** "외부에서 변환된 공식 양식(.docx→Docs)은 셀 단위 직접 편집 대신, 검수 통과본을 **마크다운 import로 새 Doc 생성**"을 기본 절차로 명문화. 부득이 편집 시 `inspect_doc_structure`에서 **tab_id를 반드시 확인**하도록 체크리스트화.
2. **MCP 인증 트러블슈팅 문서화:** `docs/GOOGLE-WORKSPACE-MCP.md`에 (a) 시크릿 타입/redirect_uri, (b) 테스트 유저 등록, (c) attachments 경로 제약 + `content` 파라미터 대안을 FAQ로 추가.
3. **상태 핸드오프 표준:** 작성→검수→반영 사이에 "검수 판정(✅/🔴, 사유)"을 구조화해 전달해, 다음 에이전트가 재질문 없이 진행하도록.
4. **작성본 파일 우선 산출 워크플로우:** 검수·반영 전에 산출물을 먼저 평문(.md) 파일로 만들고 그걸 검수/임포트하는 흐름을 표준으로. (이번에 이 방식이 가장 매끄러웠음)
5. **부분 업데이트 가이드:** 소규모 수정은 `find_and_replace_doc` 등 부분 편집 우선, 구조 변경 시에만 전체 재import.

---

## 5. 결과물 (이번 세션 산출)

- 이카운트 서버개발(신입) 지원서 작성본 — 검수 통과, Drive(mcp폴더) Google Doc + 로컬 `.md`
- GitHub 프로필 README 개편 — 사실 기반 정성 표현으로 push 완료
- 프로젝트 경험 4건(AllEat·Happy:Re·UNISON·agents-zip), 자소서 2문항, IT·AI 역량 8항목 정리

> 종합: **콘텐츠 품질·거버넌스는 강점**, **Drive/양식 처리 파이프라인의 안정성**이 다음 개선 1순위.
