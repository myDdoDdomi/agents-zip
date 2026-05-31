---
name: db-design
description: DB 설계서(ERD)를 작성/수정합니다. "DB 설계", "ERD", "테이블 명세", "데이터 모델" 요청 시 사용. 사용법 /db-design [도메인명]
---

# /db-design — DB 설계서(ERD)

`data-modeler` 에이전트를 사용해 **DB 설계서**를 만듭니다.

## 절차
1. `templates/DB설계서.md` 구조를 확인한다.
2. 기능명세·API명세를 참조해 엔티티/관계를 도출한다.
3. `data-modeler`에게 위임: Mermaid `erDiagram` + 테이블별 컬럼 명세(타입·키·제약)·인덱스·삭제정책, 엔티티 추적.
4. 완료 후 `/doc-review` → `/drive-sync` 안내.

인자가 없으면 대상 도메인을 먼저 묻는다.
