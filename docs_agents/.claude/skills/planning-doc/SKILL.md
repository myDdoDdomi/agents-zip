---
name: planning-doc
description: 기획서를 작성/수정합니다. "기획서", "서비스 기획", "기능 기획" 요청 시 사용. 사용법 /planning-doc [주제]
---

# /planning-doc — 기획서

`doc-planner` 에이전트를 사용해 **기획서**를 만듭니다.

## 절차
1. `templates/기획서.md`를 읽어 구조를 확인한다.
2. (있으면) Drive `config/drive-config.md`의 `01_기획` 폴더에서 관련 상위 문서를 참조한다.
3. `doc-planner` 에이전트에게 위임해 초안을 작성한다. 정보 부족 시 사용자에게 질문한다.
4. 초안 완성 후 사용자에게 다음을 안내한다: `/doc-review`(검수) → `/drive-sync`(Drive 반영).

인자(`$ARGUMENTS`)가 있으면 주제로 사용하고, 없으면 무엇을 기획할지 먼저 묻는다.
