---
name: blog-post
description: 블로그·롱폼 콘텐츠를 작성합니다. "블로그", "아티클", "롱폼", "콘텐츠 글" 요청 시 사용. 사용법 /blog-post [주제]
---

# /blog-post — 블로그·롱폼 콘텐츠

`content-writer` 에이전트를 사용해 **블로그·롱폼**을 작성합니다.

## 절차
1. `templates/블로그포스트.md` 구조를 확인한다.
2. (있으면) `seo-specialist`의 SEO 브리프와 `market-researcher` 근거를 참조한다(없으면 `/seo-brief` 먼저 권장).
3. `content-writer`에게 위임해 **개요(제목·H2/H3) 먼저 합의** 후 본문을 작성한다. 사실 주장에는 근거 표시.
4. 메타 타이틀·디스크립션·내부링크 제안을 달고, 완료 후 `/copy-review` 검수를 안내한다.

인자(`$ARGUMENTS`)가 있으면 주제로 사용하고, 없으면 먼저 묻는다.
