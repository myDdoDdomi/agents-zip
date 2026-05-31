---
name: seo-brief
description: SEO 콘텐츠 브리프와 온페이지 최적화안을 만듭니다. "SEO", "키워드 리서치", "콘텐츠 브리프", "검색 최적화" 요청 시 사용. 사용법 /seo-brief [주제/페이지]
---

# /seo-brief — SEO 브리프

`seo-specialist` 에이전트를 사용해 **SEO 브리프**를 만듭니다.

## 절차
1. `templates/SEO브리프.md` 구조를 확인한다.
2. 대상 주제/페이지와 비즈니스 목표를 확인한다.
3. `seo-specialist`에게 위임해 키워드·검색 의도(표), 콘텐츠 구조(H2·PAA·내부링크), 메타 가이드를 작성한다.
   검색량·난이도 추정은 `(추정)`으로 표시.
4. 이 브리프를 `/blog-post`(content-writer)의 입력으로 넘긴다. 기술 SEO 구현은 `dev_agents` 핸드오프.

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 묻는다.
