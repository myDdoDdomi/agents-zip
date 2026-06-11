---
name: device-threat-model
description: AIoT 디바이스·연결·클라우드 경계의 STRIDE 위협모델을 만들고 설계 보안을 검토합니다. "위협모델", "STRIDE", "디바이스 보안 검토", "secure boot/OTA/키 관리", "보안 위협 분석" 요청 시 사용. 사용법 /device-threat-model [대상 시스템/경계]
---

# /device-threat-model — 디바이스 보안 위협모델(STRIDE)

`device-security-engineer` 에이전트를 사용해 **STRIDE 위협모델과 설계 보안 검토**를 만듭니다.

## 절차
1. `templates/위협모델-STRIDE.md` 구조를 확인한다.
2. 대상 아키텍처·연결·클라우드 설계를 읽어 자산·신뢰경계·데이터흐름을 파악한다.
3. `device-security-engineer`에게 위임해 STRIDE 6범주별 위협·완화책을 도출하고,
   secure boot·키 관리·OTA 무결성·전송 암호화·프로비저닝 보안을 점검한다.
4. **모든 발견을 심각도(🔴/🟠/🟡)·신뢰도와 함께** 보고한다(높은 것만 거르지 않음).

## 주의
- read-only 위협모델·검토만 한다. **정식 침투 테스트(펜테스트)는 `dev_agents` `/pentest`** 핸드오프(CLAUDE.md §7).
- 관련 표준(ETSI EN 303 645, NIST IoT, IEC 62443)은 참조로 표기(`(확인 필요)`).

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 묻는다.
