---
name: aiot-architecture
description: AIoT 시스템 아키텍처(계층 구조·기술스택·엣지/클라우드 배치·데이터흐름)를 설계합니다. "AIoT 아키텍처", "시스템 설계", "전체 구조 잡아줘", "엣지 vs 클라우드 결정" 요청 시 사용. 사용법 /aiot-architecture [제품/도메인명]
---

# /aiot-architecture — AIoT 시스템 아키텍처 설계

`aiot-architect` 에이전트를 사용해 **AIoT 시스템 아키텍처**를 설계합니다.

## 절차
1. `templates/아키텍처설계서.md` 구조를 확인한다.
2. **요구·제약 우선**: 도메인·제약(전력/메모리/대역폭/레이턴시/BOM/환경)이 안 잡혔으면
   먼저 `requirements-analyst`로 요구·제약을 도출한다.
3. `aiot-architect`에게 위임해 계층 구조(디바이스–엣지–게이트웨이–클라우드)·기술스택·
   **엣지 vs 클라우드 배치**·데이터흐름을 정의하고, 모든 결정을 제약으로 정당화한다.
4. 계층별 상세 설계가 필요하면 connectivity·edge-ai·firmware·cloud로 **병렬 위임**을 제안한다.
5. 새 경계가 생기면 `device-security-engineer` 위협모델, 산출 후 `aiot-design-reviewer` 검토를 안내한다.

## 주의
- 물리/하드웨어 수치는 **사용자 확인값** 또는 `(확인 필요)`로 표기(임의 수치 금지).
- 양산 구현·정식 검증이 필요하면 `dev_agents` 핸드오프/조립을 제안한다(CLAUDE.md §7).

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 도메인·목표를 묻는다.
