# Claude Opus 4.8 — 에이전트 팀 운영 노트 (2026-06 기준)

> 이 레포의 에이전트 팀들이 **현행 모델(Claude Opus 4.8)** 에 맞춰 동작하도록 정리한 기준 문서다.
> `team-architect`와 각 팀 그룹장(CLAUDE.md)이 참조한다. (Opus 4.8 출시: 2026-05-28)
> 근거: Anthropic 공식 발표·모델 개요·마이그레이션 가이드 + Claude Code 메모리/모델 문서(§5 출처).

---

## 1. 현행 모델 라인업

| 티어 | 별칭(에이전트 `model:`) | 모델 ID | 컨텍스트 | 최대 출력 | 가격(입력/출력, 1M토큰) |
|---|---|---|---|---|---|
| **Opus** | `opus` | `claude-opus-4-8` | 1M | 128K | $5 / $25 |
| **Sonnet** | `sonnet` | `claude-sonnet-4-6` | 1M | 64K | $3 / $15 |
| **Haiku** | `haiku` | `claude-haiku-4-5` | 200K | 64K | $1 / $5 |
| **Fable 5 / Mythos 5** (참조용 신규 라인) | `fable`=`claude-fable-5` (선택적) | — | — | — | $10 / $25 (Mythos Preview 출력 $50 대비 절반) |

- 에이전트 `model:`은 **별칭**(`opus`/`sonnet`/`haiku`/`inherit`)으로 둔다. **모델 ID를 하드코딩하지 않는다** — Claude Code가
  현행 버전으로 해석하므로, 다음 모델이 나와도 정의를 안 고쳐도 된다.
- Opus 4.8는 **1M 컨텍스트를 추가요금 없이** 제공한다. Claude Code에서 **Fast 모드**(`/fast`)를 지원한다(같은 Opus, 출력만 빠름).
- **Claude Fable 5·Mythos 5 GA(2026-06-09).** SW엔지니어링·지식노동·비전에 강점. 고위험(사이버·생물·화학) 응답은 **Opus 4.8로 폴백**.
  별칭 매핑은 **기존대로 유지**(opus=`claude-opus-4-8`, sonnet=`claude-sonnet-4-6`, haiku=`claude-haiku-4-5`) — 위 표의 신규 라인은 **참조용**이며
  전 팀 에이전트 `model:` 재배정은 보류(보수적 기본값). 신모델 상시 채택 필요성이 생기면 이 라인업 기준으로 재검토한다.
- **Fable 5 구독 정책:** 구독 플랜 무료 포함은 2026-06-22까지, 6-23부터 Usage Credits 차감.

## 2. 사고(thinking)·effort — Claude Code

- Opus 4.8/4.7은 **adaptive thinking + `effort`**(`low`/`medium`/`high`/`xhigh`/`max`)로 사고 깊이를 조절한다. (`budget_tokens`는 폐지)
- Claude Code 세션 기본 effort는 `xhigh`. 단 **Opus 4.8는 지능 천장이 높아 반사적으로 `max`를 쓰지 말고 `high`에서 시작**해
  필요할 때 올린다. 장기·에이전트 작업은 `high`~`xhigh`. (effort는 세션 단위 설정이며 에이전트 `model:`과는 별개)
- 우리 팀의 **작업 깊이 3단 모드(빠르게/표준/심화)** 와 정합: *심화 = 더 높은 effort + 다관점 교차검증*, **값을 할 때만**.

## 3. Opus 4.8 행동 특성 → 에이전트·프롬프트 작성에 반영할 것 (핵심)

아래는 **모든 팀의 그룹장 프롬프트·에이전트 시스템프롬프트·`description`** 에 적용하는 작성 기준이다.

1. **지시를 더 문자 그대로 따른다.** 과장된 명령("CRITICAL: 반드시/무조건/항상")은 **과잉발동**을 부른다 →
   담백하게 **"…할 때 X 한다"** 형태로. 정확·구체적 지시가 모호한 일반화보다 낫다.
2. **위임·도구·메모리를 보수적으로 쓴다.** 알아서 서브에이전트 위임/검색/파일 메모리 사용을 **덜 한다** →
   **"언제 쓰는지"를 명시**한다. 에이전트 `description`과 도구 설명에 **"~할 때 호출"** 트리거를 박으면 호출률이 오른다(측정상 향상).
   그룹장 루프엔 *"작업 전 MEMORY 확인 / 학습 발견 시 기록"*, *"여러 항목으로 갈라지면 서브에이전트 병렬"* 을 명시.
3. **질문이 잦다(더 신중).** 사소한 결정(이름·기본값·동등 대안)까지 되묻는 경향 → 그룹장 프롬프트에
   *"사소한 선택은 합리적으로 정하고 기록만 — 범위 변경·파괴적 작업만 확인"* 를 넣어 ask-rate를 낮춘다(과한 확인 방지).
4. **나레이션이 늘었다.** 도구 호출 사이 설명·말미 요약이 길어짐 → **강제 진행보고 스캐폴딩 제거**.
   너무 수다스러우면 *"도구 호출 사이 기본은 침묵 — 발견/전환/막힘만 한 줄"* 지시.
5. **문체가 따뜻하고 덜 hedge.** 4.7의 직설을 보정하려 넣었던 톤 지시는 재검토(이제 과교정이 될 수 있음).
6. **thinking을 끈 채 장황해질 수 있음** → adaptive thinking을 켜두거나 *"최종 답만, 과정 설명 생략"* 지시.
7. **장기 작업은 첫 턴에 풀스펙 + 높은 effort.** Claude Code `/goal`로 방향을 고정한다. 모호한 다턴보다
   **명확한 단일 스펙**이 효율·정확 모두 유리(4.8는 장기 자율 실행이 강함).
8. **코드리뷰**: "고심각만 보고/보수적으로" 필터를 문자 그대로 따라 recall이 낮아 *보일* 수 있음 →
   *"모든 발견을 신뢰도·심각도와 함께 보고, 필터는 후단계"* 로 분리(dev 팀 리뷰·QA 팀에 적용).

## 4. 이 레포 적용 요약

- **team-architect**: §3을 새 에이전트의 `description`·시스템프롬프트 표준으로 적용(§산출물 2·3·품질 체크리스트). 모델 배분은 별칭 유지.
- **각 팀 그룹장(CLAUDE.md)**: "🧩 모델·프롬프트 기준(Opus 4.8)" 섹션으로 위임/메모리/ask-rate/나레이션 기준 반영.
- **MEMORY.md(학습층)**: 4.8가 파일 메모리를 덜 쓰므로, 그룹장 루프의 *"작업 전 MEMORY 확인 / 학습 기록"* 으로 활용을 끌어올린다(이미 메모리 운영 블록에 반영).

## 5. 출처 (Sources)

- [Introducing Claude Opus 4.8 — Anthropic](https://www.anthropic.com/news/claude-opus-4-8)
- [Claude Opus — Anthropic](https://www.anthropic.com/claude/opus)
- [Models overview — Claude API Docs](https://platform.claude.com/docs/en/about-claude/models/overview)
- Migration guide — *Migrating to Opus 4.8 / 4.7* 섹션 (claude-api 스킬 내장 레퍼런스 기준)
- [How Claude remembers your project — Claude Code Docs](https://code.claude.com/docs/en/memory)
