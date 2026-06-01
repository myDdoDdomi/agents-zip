---
name: insight-analysis
description: 인사이트 분석을 수행합니다. "인사이트", "분석", "시사점", "기회 도출", "패턴 분석" 요청 시 사용. 사용법 /insight-analysis [입력 자료·주제]
---

# /insight-analysis — 인사이트 분석

`insight-analyst` 에이전트를 사용해 **레퍼런스 리서치(또는 사용자 자료)를 인사이트로 전환**합니다.
(발견사실 → 패턴 → 인사이트 → 기회/리스크 → 우리 제품 적용 제안 → 우선순위)

## 절차
1. `templates/인사이트분석.md`를 읽어 구조를 확인한다.
2. 입력 자료를 받는다: `/reference-research`로 만든 레퍼런스 리서치 또는 사용자가 준 자료.
3. `insight-analyst`에게 위임해 분석한다.
   - 인사이트마다 근거(어느 발견사실에서 왔는지) 연결. 추정은 `(가설)`, 미검증은 `(확인 필요)`로 구분.
4. 초안 완성 후 안내한다: `/planning-doc`(기획서) → `/doc-review` → `/drive-sync`.

> 경계: *확정 기획 → 요구사항(FR/NFR)* 분해는 `/requirements-doc`(requirements-analyst)의 몫이다.
> *시장규모·GTM* 분석은 `marketing_agents`로 핸드오프한다.

인자(`$ARGUMENTS`)가 있으면 입력 자료·주제로 사용하고, 없으면 무엇을 분석할지 먼저 묻는다.
