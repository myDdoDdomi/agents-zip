---
name: feedback-agents
description: 이 QA 팀이 대상 프로젝트에서 한 작업을 회고해 본부(team-architect)에 전달할 구조화된 피드백 보고서(feedback.md)를 만듭니다. "회고", "피드백 보고서", "QA 팀 개선점 정리" 요청 시 사용. 사용법 /feedback-agents [회고할 작업 주제(선택)]
---

# /feedback-agents — QA 팀 작업 회고·피드백 보고

`feedback-reporter` 에이전트를 사용해 **이 QA 팀이 대상 프로젝트에서 한 작업을 회고**하고,
본부(team-architect)가 QA 팀 정의·문서를 바로 개선할 수 있도록 구조화된 `feedback.md`를 산출합니다.

> **모델 B(복사-라이브러리) 안내:** 이 스킬은 `agents/feedback-reporter.md`와 함께 **대상 프로젝트로
> 복사**해 씁니다. 스킬을 쓰려면 `QA_agents/skills/feedback-agents/`를 대상 프로젝트의
> `.claude/skills/`로 복사하세요. 스킬을 복사하지 않아도 **`feedback-reporter` 에이전트를 직접 호출**
> ("feedback-reporter로 이번 QA 작업을 회고해줘")하면 동일하게 동작합니다.

## 절차
1. **범위 결정:** 대상 프로젝트의 `feedback/`에서 가장 최근 피드백 보고서를 찾는다.
   있으면 그 시점 이후, 없으면 QA 작업 전체가 회고 범위.
2. `feedback-reporter`에게 위임: git 로그·세션 맥락·산출물 흔적을 best-effort로 수집 →
   `현상 → 영향 → 권장 업데이트(대상 파일)` + 심각도(🔴/🟠/🟡)로 이슈 도출.
3. 표준 6섹션 양식(① 요약 한 줄 ② 우선순위 요약 표 ③ 이슈 상세 ④ 잘 작동한 점(유지)
   ⑤ 본부 액션 아이템 ⑥ 참고)으로 `feedback.md` 작성.
4. **저장:** 대상 프로젝트의 `feedback/`에 날짜 포함 파일명으로 저장(폴더 없으면 생성).
   생성한 보고서는 원본 `QA_agents` 레포의 본부(team-architect)에 전달한다.
5. 저장 경로와 핵심 이슈 요약을 보고한다.

## 주의
- **best-effort 회고.** 가용 신호(git·세션·산출물 흔적) 기반이며 세션 외 작업은 누락될 수 있음을
  보고서 머리말에 1줄 명시한다. 근거 불확실 항목은 추측하지 말고 `(확인 필요)`로 표기.
- 팀 개선 심각도와 *발견 버그의 Sev 등급*을 혼동하지 않는다.
- 테스트·검증·외부 변경을 하지 않는다(회고 + feedback 파일 1개만 작성).

인자(`$ARGUMENTS`)가 있으면 회고할 작업 주제로 사용하고, 없으면 전체 범위로 회고한다.
