---
name: meeting-notes
description: 회의록을 작성/정리합니다. "회의록", "미팅 정리", "논의 정리" 요청 시 사용. 사용법 /meeting-notes [회의 주제 또는 메모 붙여넣기]
---

# /meeting-notes — 회의록

`tech-writer` 에이전트를 사용해 **회의록**을 만듭니다.

## 절차
1. `templates/회의록.md` 구조를 확인한다.
2. 사용자가 붙여넣은 메모/녹취/논의 내용을 입력으로 받는다(없으면 핵심을 질문).
3. `tech-writer`에게 위임: 안건별 논의→결정→액션아이템(담당·기한), 결정/미결 분리.
4. 완료 후 `/doc-review` → `/drive-sync` 안내.
