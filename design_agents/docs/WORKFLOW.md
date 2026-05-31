# 작업 워크플로우

디자인 작업은 5단계 루프를 따릅니다. 그룹장(`CLAUDE.md`)이 라우팅·종합·게이트를 맡습니다.

```
① 발상(Ideate)    → /ideate, /prototype        (ideation-prototyper)
② 정의(Define)    → /design-tokens, /design-system (design-system-architect)
③ 생성/추출(Make) → /figma-sync                 (figma-operator, 쓰기=사람 승인)
④ 검토(Review)    → /a11y-review                (accessibility-ux-reviewer)
⑤ 정합(Connect)  → /code-connect               (code-connect-mapper)
```

## 협업 흐름

```
요청
  │
  ▼
[design-lead] ── 전략·범위·재사용 결정·작업 분배
  │
  ├──▶ [ideation-prototyper] ── 컨셉·프로토타입
  ├──▶ [design-system-architect] ── 토큰·시스템 (기존 재사용 우선)
  ├──▶ [accessibility-ux-reviewer] ── 접근성·UX 검토
  │
  ├──▶ [figma-operator] ── Figma 추출/생성 (쓰기 = 사람 승인)
  ├──▶ [code-connect-mapper] ── 디자인-코드 정합·drift
  │
  ▼
[design-lead] ── 종합 · 품질 게이트 판정
```

## 깊이 모드 (효율)
- **빠르게**: 단일 화면 컨셉, 간단 검토 → 담당 1명.
- **표준(기본)**: 일반 작업 → 담당 1~2명 + 그룹장 요약.
- **심화**: 디자인 시스템 전면 개편·핵심 플로우 재설계 → 다관점 병렬 + 교차검증(그룹장이 제안·승인).

## 게이트(사람 승인) 항목
- Figma 쓰기(노드 생성·토큰 push), Code Connect 대상 레포 커밋, 파일 덮어쓰기.
