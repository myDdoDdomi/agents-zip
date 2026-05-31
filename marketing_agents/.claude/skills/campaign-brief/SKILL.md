---
name: campaign-brief
description: 마케팅 캠페인 브리프를 작성합니다. "캠페인 기획", "캠페인 브리프", "마케팅 캠페인" 요청 시 사용. 사용법 /campaign-brief [캠페인/제품명]
---

# /campaign-brief — 캠페인 브리프

`marketing-lead` 에이전트를 사용해 **캠페인 브리프**를 만듭니다.

## 절차
1. `templates/캠페인브리프.md` 구조를 확인한다.
2. (있으면) `market-researcher` 리서치·기존 포지셔닝을 참조한다.
3. `marketing-lead`에게 위임해 목표/KPI·타깃·핵심 메시지·채널 믹스·산출물·일정을 정리한다.
   정보 부족 시 사용자에게 핵심부터 질문한다.
4. 완료 후 채널별 산출물을 담당 에이전트에 분배하고, 검수는 `/copy-review`로 안내한다.

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 무엇을 위한 캠페인인지 먼저 묻는다.
