# Code Connect 매핑: <컴포넌트/디자인시스템명>

> 작성: `code-connect-mapper` · 대상 코드베이스: (경로/레포) · 최종수정: YYYY-MM-DD

## 1. 매핑 개요
| Figma 컴포넌트 | 코드 컴포넌트(경로) | 프레임워크 | 상태 |
|---|---|---|---|
| Button | `src/ui/Button.tsx` | React | 매핑됨/대기 |

## 2. Prop · Variant 매핑
| Figma 속성/변형 | 코드 prop | 값 대응 | 비고 |
|---|---|---|---|
| Variant=Primary | `variant="primary"` | | |
| State=Disabled | `disabled` | boolean | |
| Size=Large | `size="lg"` | | |

## 3. 토큰 대응
| Figma 변수 | 코드 토큰 | Tailwind key | 일치? |
|---|---|---|---|
| `color/action/primary` | `tokens.color.action.primary` | `extend.colors.action.primary` | ✅/❌ |

> Tailwind를 안 쓰면 Tailwind key 열은 `N/A`. v4 채택 시 `extend` 키 대신 CSS 변수명(`@theme --var-name`, 예: `--color-action-primary`)으로 기입. 1:1 어긋나면 §4 Drift에 적고 동기화 방향을 제안.

## 4. Drift (불일치) · 해소
| 항목 | Figma | 코드 | 해소 방향(출처) |
|---|---|---|---|
| 누락 변형 | Ghost 있음 | 없음 | 코드에 추가 / Figma에서 제거 |

## 5. Code Connect 스니펫 (제안)
```ts
// 예시 — 대상 레포 커밋은 사람 승인(via design-lead) 후
import figma from '@figma/code-connect'
import { Button } from './Button'

figma.connect(Button, '<FIGMA_NODE_URL>', {
  props: {
    variant: figma.enum('Variant', { Primary: 'primary', Secondary: 'secondary' }),
    disabled: figma.boolean('Disabled'),
  },
  example: (props) => <Button {...props} />,
})
```

## 6. 미확인
- `(확인 필요: 코드 prop명은 실제 소스 기준 재확인)`
