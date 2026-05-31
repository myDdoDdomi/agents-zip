---
name: release-notes
description: 릴리즈노트/변경 이력을 작성합니다. "릴리즈노트", "변경 이력", "패치노트" 요청 시 사용. 사용법 /release-notes [버전 또는 변경 목록]
---

# /release-notes — 릴리즈노트

`tech-writer` 에이전트를 사용해 **릴리즈노트**를 만듭니다.

## 절차
1. `templates/릴리즈노트.md` 구조를 확인한다.
2. 변경 목록(커밋/이슈/기능 변화)을 입력으로 받는다(없으면 질문).
3. `tech-writer`에게 위임: 버전·날짜, ✨신규/🔧개선/🐛버그수정/⚠️주의로 분류, 사용자 영향 중심 서술.
4. 완료 후 `/doc-review` → `/drive-sync` 안내.
