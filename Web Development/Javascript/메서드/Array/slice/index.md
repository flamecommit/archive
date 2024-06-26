# Array.prototype.slice()

## 요약

- 설명 : 호출 배열의 구획을 추출하고 새 배열을 반환합니다.
- 메서드 분류 : 범용 메서드
- 원본 수정 : X
- 반환 타입 : Array

## 개요

`slice()` 메서드는 어떤 배열의 `begin` 부터 `end` 까지 (`end` 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환합니다. 원본 배열은 바뀌지 않습니다.

```js
const number = [0, 1, 2, 3, 4];

console.log(number.slice(2)); // [2, 3, 4]

console.log(number.slice(2, 4)); // [2, 3]

console.log(number.slice(1, 5)); // [1, 2, 3, 4]

console.log(number.slice(-2)); // [3, 4]

console.log(number.slice(2, -1)); // [2, 3]

console.log(number.slice()); // [0, 1, 2, 3, 4]
```

## 구문

```js
arr.slice([begin[, end]])
```

### 매개변수

- `begin`(Optional) : 0을 시작으로 하는 추출 시작점에 대한 인덱스를 의미합니다. 음수 인덱스는 배열의 끝에서부터의 길이를 나타냅니다. `slice(-2)`는 배열에서 마지막 두 개의 엘리먼트를 추출합니다. `begin`이 `undefined`인 경우에는, 0번 인덱스부터 `slice` 합니다. `begin`이 배열의 길이보다 큰 경우에는, 빈 배열을 반환합니다.
- `end`(Optional) : 추출을 종료 할 0 기준 인덱스입니다. `slice`는 `end`인덱스를 제외하고 추출합니다. 예를 들어 `slice(1, 4)`는 두번째 요소부터 네번째 요소까지 (1, 2 및 3을 인덱스로 하는 요소) 추출합니다. 음수 인덱스는 배열의 끝에서부터의 길이를 나타냅니다. 예를들어 `slice(2, -1)`는 세번째부터 끝에서 두번째 요소까지 추출합니다. `end`가 생략되면 `slice()`는 배열의 끝(`arr.length`)까지 추출합니다.

만약 `end` 값이 배열의 길이보다 크다면, `slice()`는 배열의 끝까지 (`arr.length`) 추출합니다.

### 반환 값

추출한 요소를 포함한 새로운 배열.

## 설명

`slice()`는 원본을 대체하지 않습니다. 원본 배열에서 요소의 얕은 복사본을 반환합니다. 원본 배열의 요소는 다음과 같이 반환 된 배열에 복사됩니다.

- 객체 참조(및 실제 객체가 아님)의 경우, `slice()`는 객체 참조를 새 배열로 복사합니다. 원본 배열과 새 배열은 모두 동일한 객체를 참조합니다. 참조 된 객체가 변경되면 변경 내용은 새 배열과 원래 배열 모두에서 볼 수 있습니다.
- `String` 및 `Number` 객체가 아닌 문자열과 숫자의 경우 `slice()`는 문자열과 숫자를 새 배열에 복사합니다. 한 배열에서 문자열이나 숫자를 변경해도 다른 배열에는 영향을 주지 않습니다.

새 요소를 두 배열 중 하나에 추가해도 다른 배열은 영향을 받지 않습니다.