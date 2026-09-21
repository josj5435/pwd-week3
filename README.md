# TypeScript 계산기

HTML과 CSS로 화면을 구성하고 TypeScript로 계산 기능을 구현한 기본 계산기입니다. 사칙연산뿐만 아니라 소수점 입력, 숫자 삭제, 부호 변경, 퍼센트 계산, 계산식 표시, 오류 처리 기능을 제공합니다.

## Step 7 동작 확인 결과


| 입력                  | 확인 결과       | 담당 파일 및 함수                                                                 |
| ------------------- | ----------- | -------------------------------------------------------------------------- |
| `12 → + → 3 → =`    | `15`        | `operations.ts`의 `add()`, `calculate()` / `calculator.ts`의 `equals()`      |
| `12 → − → 3 → =`    | `9`         | `operations.ts`의 `subtract()`, `calculate()` / `calculator.ts`의 `equals()` |
| `12 → × → 3 → =`    | `36`        | `operations.ts`의 `multiply()`, `calculate()` / `calculator.ts`의 `equals()` |
| `12 → ÷ → 3 → =`    | `4`         | `operations.ts`의 `divide()`, `calculate()` / `calculator.ts`의 `equals()`   |
| `0.1 → + → 0.2 → =` | `0.3`       | `add()`, `calculate()`, `formatNumber()`, `render()`                       |
| `123 → ⌫`           | `12`        | `calculator.ts`의 `handleKey()` 삭제 처리                                       |
| `5 → +/−`           | `-5`        | `calculator.ts`의 `handleKey()` 부호 변경 처리                                    |
| `50 → %`            | `0.5`       | `calculator.ts`의 `handleKey()`, `setResult()`                              |
| `6110000`           | `6,110,000` | `app.ts`의 `formatDisplay()`, `render()`                                    |

`operations.ts`에서는 실제 사칙연산을 처리하며, `calculator.ts`에서는 현재 입력값과 연산 상태를 관리합니다. 화면에 숫자를 표시할 때에는 `app.ts`의 `render()`가 호출됩니다.

### 오류 처리와 복구

`12 → ÷ → 0 → =`을 입력했을 때 결과 화면에 `Error`가 나타나고, 아래에는 `0으로 나눌 수 없습니다.`라는 메시지가 표시되었습니다.

오류는 다음 순서로 처리됩니다.

1. `operations.ts`의 `divide()`가 0으로 나누는 상황을 확인하고 오류를 발생시킵니다.
2. `calculator.ts`의 `handleKey()`가 오류를 받아 `state.error`에 저장합니다.
3. `app.ts`의 `render()`가 `Error`와 오류 메시지를 화면에 표시합니다.
4. 오류가 표시된 상태에서 숫자 `7`을 누르면 `inputDigit()`이 `clear()`를 호출하여 오류 상태를 초기화하고 새로운 입력을 시작합니다.

### 연속 입력

| 입력                      | 확인 결과                     | 담당 파일 및 함수                               |
| ----------------------- | ------------------------- | ---------------------------------------- |
| `2 → + → 3 → × → 4 → =` | 입력 순서대로 계산되어 `20`         | `calculator.ts`의 `selectOperator()`      |
| `2 → + → × → 3 → =`     | 마지막 연산자인 곱셈이 적용되어 `6`     | `calculator.ts`의 `selectOperator()`      |
| `200 → + → 10 → % → =`  | `10`이 `0.1`로 변환되어 `200.1` | `handleKey()`, `setResult()`, `equals()` |
| `2 → + → =`             | 두 번째 숫자를 기다리며 `2` 유지      | `calculator.ts`의 `equals()` 조건문          |
| `2 → + → 3 → = → =`     | 반복 계산 없이 `5` 유지           | `calculator.ts`의 `equals()` 조건문          |

이 계산기는 연산자 우선순위를 적용하지 않고 사용자가 입력한 순서대로 계산합니다. 따라서 `2 + 3 × 4`는 먼저 `2 + 3`을 계산한 다음 `5 × 4`를 계산하여 `20`이 됩니다.