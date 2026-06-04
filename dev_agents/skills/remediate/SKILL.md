---
name: remediate
description: 레드팀 침투 테스트 리포트를 기반으로 방어(수정·패치)를 시작합니다. remediation-lead로 위임해 발견을 우선순위화하고 기존 dev 에이전트로 수정을 분배·리뷰·재검증합니다. "보안 패치", "취약점 방어", "리포트 기반 수정" 요청 시 사용. 사용법 /remediate [리포트 경로/대상(선택)]
---

# /remediate — 침투 테스트 리포트 기반 방어(블루팀)

`remediation-lead` 오케스트레이터를 통해 레드팀 **침투 테스트 리포트**의 발견을 **방어(수정·패치)**로
전환합니다. 발견을 우선순위화하고, 기존 dev 에이전트(`implementer`·`debugger`·`refactorer`·
`migration-engineer`·`code-reviewer`)로 수정을 분배·리뷰한 뒤, 레드팀에 **재검증**을 요청해 퍼플팀
루프(red → report → **blue**)를 닫습니다.

> **모델 B(복사-라이브러리) 안내:** 이 스킬은 `agents/remediation-lead.md`(및 dev 에이전트들)와 함께
> **대상 프로젝트로 복사**해 씁니다. 스킬을 쓰려면 `dev_agents/skills/remediate/`를 대상 프로젝트의
> `.claude/skills/`로 복사하세요. 복사하지 않아도 **`remediation-lead`를 직접 호출**("remediation-lead로
> 이 리포트 방어 시작해줘")하면 동일하게 동작합니다.

## 절차

1. **리포트 수신·요약:** `remediation-lead`가 `redteam-reporter`의 리포트(또는 지정 경로)를 읽고
   발견을 **심각도×악용가능성×영향**으로 우선순위 표로 요약합니다. 오탐/미검증 상태를 반영합니다.
2. **수정안 설계:** 각 발견에 **근본 수정**(증상 가리기 금지)을 설계합니다 — 입력 검증·권한 체크·
   안전한 API·의존성 업그레이드·구성 변경 등.
3. **위임·통합:** 코드 수정 → `implementer`, 근본원인 → `debugger`, 구조 개선 → `refactorer`,
   의존성/스키마/IaC → `migration-engineer`(승인 게이트), 보안 리뷰 → `code-reviewer`. 독립 수정은 병렬.
4. **재검증 루프:** 수정이 끝나면 레드팀(`exploit-verifier`/`redteam-lead`)에 **재검증**을 요청해
   발견이 실제로 닫혔는지 확인합니다. 체계적 보안 테스트가 필요하면 QA로 핸드오프.
5. **추적·승인:** 발견별 진행 상태를 추적하고, PR 머지·운영 변경(의존성/스키마/IaC)은 **사람 승인**을 받습니다.

## 주의
- **근본 수정.** 증상만 가리지 않습니다. 시크릿이 노출됐던 발견은 **키 회전(rotation)**까지 권고합니다.
- **권한 경계.** 의존성/스키마/IaC 변경은 `migration-engineer`·승인 게이트로 분리합니다.
- **Human-in-the-loop.** PR 머지·운영 변경은 사람 최종 승인 전제.

인자(`$ARGUMENTS`)가 있으면 리포트 경로/대상으로 사용하고, 없으면 `remediation-lead`가 리포트를 먼저 찾습니다.
