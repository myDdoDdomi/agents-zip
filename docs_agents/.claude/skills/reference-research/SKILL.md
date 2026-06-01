---
name: reference-research
description: 레퍼런스 리서치를 수집/정리합니다. "레퍼런스", "벤치마크", "사례조사", "리서치", "아이디어 수집" 요청 시 사용. 사용법 /reference-research [대상·주제]
---

# /reference-research — 레퍼런스 리서치

`reference-researcher` 에이전트를 사용해 **기획 전 단계 레퍼런스 리서치**를 만듭니다.
(경쟁/유사 제품 사례 · 기능·UX 벤치마크 · 인터랙션 패턴 · 업계 동향 · 아이디어 풀)

## 절차
1. `templates/레퍼런스리서치.md`를 읽어 구조를 확인한다.
2. 리서치 범위(대상 제품군·비교 기준·조사 질문)를 사용자와 합의한다. 모호하면 1~2개만 질문.
3. `reference-researcher`에게 위임해 WebSearch/WebFetch로 **출처와 함께** 사례·벤치마크·아이디어를 수집한다.
   - 모든 사실에 출처 URL·확인 날짜. 불확실하면 `(확인 필요)`. 추측 순위 금지.
4. 초안 완성 후 안내한다: `/insight-analysis`(인사이트 분석) → `/planning-doc`(기획서) → `/doc-review` → `/drive-sync`.

> 경계: *시장규모·세그먼트·GTM* 같은 마케팅 리서치는 `marketing_agents`로 핸드오프한다.

인자(`$ARGUMENTS`)가 있으면 리서치 대상·주제로 사용하고, 없으면 무엇을 조사할지 먼저 묻는다.
