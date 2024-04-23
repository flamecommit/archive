# Array.prototype.fill()

- 설명 : 배열의 인덱스 범위 내에 있는 모든 요소를 정적 값으로 변경합니다. 그 후 수정된 배열을 반환합니다.
- 메서드 분류 : 변경 메서드
- 원본 수정 : O
- 반환 타입 : Array

```js
const array1 = [1, 2, 3, 4];
const array2 = array1.fill(0, 2, 4);

console.log(array1, array2);
```

## 구문

```js
fill(value)
fill(value, start)
fill(value, start, end)
```

### 매개변수

- `value` : 배열을 채울 값입니다. 배열의 모든 요소는 정확히 이 값이 될것입니다. `value`가 객체인 경우, 배열의 각 슬롯은 해당 객체를 참조합니다.
- `start` (Optional) : 채우기를 시작할 0 기반 인덱스입니다.
- `end` (Optional) : 채우기를 끝낼 0 기반 인덱스입니다. `fill()`은 `end`까지 채우며, `end`는 포함하지 않습니다.

### 반환 값

`value`로 채워진 변경된 배열입니다.