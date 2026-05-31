---
name: social-calendar
description: 소셜미디어·숏폼 콘텐츠 캘린더와 포스트를 만듭니다. "소셜", "SNS", "콘텐츠 캘린더", "숏폼", "릴스/쇼츠 스크립트" 요청 시 사용. 사용법 /social-calendar [캠페인/기간]
---

# /social-calendar — 소셜·숏폼 캘린더

`social-media-strategist` 에이전트를 사용해 **소셜 캘린더·포스트**를 만듭니다.

## 절차
1. `templates/소셜캘린더.md` 구조를 확인한다.
2. 캠페인 브리프·타깃·대상 채널·기간을 확인한다.
3. `social-media-strategist`에게 위임해 기간별 캘린더(표: 날짜·채널·포맷·후크·CTA)와 핵심 포스트
   카피·숏폼 스크립트를 작성한다. 채널 규격(`docs/CHANNEL-GUIDE.md`) 준수.
4. 완료 후 `/copy-review` 검수를 안내한다. **실제 발행·예약은 사람 승인 전제**(직접 게시하지 않음).

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 캠페인·채널을 먼저 묻는다.
