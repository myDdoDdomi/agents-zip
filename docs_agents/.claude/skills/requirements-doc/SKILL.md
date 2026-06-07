---
name: requirements-doc
description: 요구사항 정의서(SRS)를 작성/수정합니다. "요구사항", "SRS", "기능/비기능 요구사항" 요청 시 사용. 사용법 /requirements-doc [주제]
---

# /requirements-doc — 요구사항 정의서

`requirements-analyst` 에이전트를 사용해 **요구사항 정의서**를 만듭니다.

## 절차
1. `templates/요구사항정의서.md` 구조를 확인한다.
2. 상위 기획서가 Drive(`01_기획`)에 있으면 참조해 요구사항을 도출한다.
3. `requirements-analyst`에게 위임: FR/NFR 식별, ID 부여, 인수기준·우선순위·추적표 작성.
   요구사항 표(FR·NFR)의 **맨 우측에 `검토 항목` 열을 상시 포함**한다(리뷰어가 항목별로 확인·체크하며 보도록 — 기본값).
4. 완료 후 `/doc-review` → `/drive-sync` 안내.

인자가 없으면 대상 기능/제품과 기획서 위치를 먼저 묻는다.
