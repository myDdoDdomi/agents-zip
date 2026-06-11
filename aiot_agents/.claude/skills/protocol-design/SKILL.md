---
name: protocol-design
description: AIoT 연결성·통신 프로토콜·네트워크 토폴로지를 설계합니다. "프로토콜 선택", "BLE/Wi-Fi/LoRa/Matter 비교", "통신 설계", "MQTT/CoAP 설계", "토폴로지" 요청 시 사용. 사용법 /protocol-design [디바이스/네트워크 컨텍스트]
---

# /protocol-design — 연결성·프로토콜 설계

`connectivity-protocol-engineer` 에이전트를 사용해 **프로토콜·연결성**을 설계합니다.

## 절차
1. `templates/프로토콜설계서.md` 구조를 확인한다.
2. 제약(거리·노드 수·전력·대역폭·레이턴시·실내외·비용)을 확인한다(없으면 1~2개 질문).
3. `connectivity-protocol-engineer`에게 위임해 무선/유선 프로토콜·토폴로지·애플리케이션 프로토콜
   (MQTT/CoAP)·페이로드·QoS·보안 전송을 설계하고, **대안 2개 이상을 비교표**로 제시한다.
4. 보안 전송(키/인증서)은 `device-security-engineer`, 전력 영향은 `firmware-embedded-engineer`와 협의.

## 주의
- 프로토콜 선택은 반드시 **제약으로 정당화**한다. 표준 버전·리전 규제는 확인값/`(확인 필요)`.

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 묻는다.
