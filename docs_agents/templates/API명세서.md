# [서비스/도메인명] API 명세서

| 항목 | 내용 |
|---|---|
| 문서 버전 | v0.1 |
| 작성자 | (이름) |
| 작성일 | YYYY-MM-DD |

## 1. 공통 규약
| 항목 | 내용 |
|---|---|
| Base URL | `https://api.example.com/v1` |
| 인증 | Bearer JWT (`Authorization: Bearer <token>`) |
| 콘텐츠 타입 | `application/json; charset=utf-8` |
| 페이징 | `?page=1&size=20` (응답에 total 포함) |
| 정렬/필터 | `?sort=createdAt,desc&status=ACTIVE` |
| 버전 정책 | URL 경로 버전(`/v1`) |
| 명명 규칙 | 리소스 복수형, camelCase 필드 |

### 공통 에러 응답
```json
{ "code": "INVALID_PARAMETER", "message": "이메일 형식이 올바르지 않습니다.", "field": "email" }
```

## 2. 엔드포인트
> 엔드포인트 단위로 반복.

### [API-001] 로그인  `POST /auth/login`
| 항목 | 내용 |
|---|---|
| 설명 | 이메일/비밀번호로 로그인 |
| 인증 | 불필요 |
| 연결 기능 | FN-001 (FR-001) |

**요청 (Body)**
| 필드 | 타입 | 필수 | 제약 | 설명 |
|---|---|---|---|---|
| email | string | Y | 이메일 형식 | |
| password | string | Y | 8~64자 | |

```json
{ "email": "user@example.com", "password": "********" }
```

**응답 200**
| 필드 | 타입 | 설명 |
|---|---|---|
| accessToken | string | JWT |
| expiresIn | number | 만료(초) |

```json
{ "accessToken": "eyJ...", "expiresIn": 3600 }
```

**상태/에러 코드**
| HTTP | code | 원인 | 해결 |
|---|---|---|---|
| 400 | INVALID_PARAMETER | 형식 오류 | 입력 확인 |
| 401 | AUTH_FAILED | 자격 불일치 | 재시도 |

## 3. 추적성
| API ID | 기능 ID | 화면 ID |
|---|---|---|
| API-001 | FN-001 | |

## 4. (선택) OpenAPI 스니펫
```yaml
# paths 일부
```
