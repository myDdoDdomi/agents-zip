---
name: api-spec
description: API 명세서를 작성/수정합니다. "API 명세", "엔드포인트 정의", "REST/GraphQL 스펙" 요청 시 사용. 사용법 /api-spec [기능/리소스명]
---

# /api-spec — API 명세서

`api-designer` 에이전트를 사용해 **API 명세서**를 만듭니다.

## 절차
1. `templates/API명세서.md` 구조를 확인한다.
2. 기능명세서·화면정의서·DB설계서를 참조해 필요한 엔드포인트를 도출한다.
3. `api-designer`에게 위임: 공통 규약 + 엔드포인트별 요청/응답(예시 JSON)/상태·에러코드/인증, API ID 추적.
4. 완료 후 `/doc-review` → `/drive-sync` 안내. (후속: `/sequence-diagram`)

인자가 없으면 대상 기능/리소스를 먼저 묻는다.
