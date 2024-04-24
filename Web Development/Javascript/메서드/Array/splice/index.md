# Array.prototype.splice()

## 요약

- 설명 : 원본 배열에서 요소를 추가 및 제거하고 제거된 요소를 반환합니다.
- 메서드 분류 : 변경 메서드
- 원본 수정 : O
- 반환 타입 : any

## 개요

`splice()` 메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.

```js
const months = ['Jan', 'March', 'April', 'June'];
const result = months.splice(1, 0, 'Feb');

console.log(months) // ["Jan", "Feb", "March", "April", "June"]
```

## 구문

```js
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```

### 매개변수

- `start` : 배열의 변경을 시작할 인덱스입니다. 배열의 길이보다 큰 값이라면 실제 시작 인덱스는 배열의 길이로 설정됩니다. 음수인 경우 배열의 끝에서부터 요소를 세어나갑니다(원점 -1, 즉 -n이면 요소 끝의 n번째 요소를 가리키며 `array.length - n`번째 인덱스와 같음). 값의 절대값이 배열의 길이 보다 큰 경우 0으로 설정됩니다.
- `deleteCount`(Optional) : 배열에서 제거할 요소의 수입니다. `deleteCount`를 생략하거나 값이 `array.length - start`보다 크면 `start`부터의 모든 요소를 제거합니다. `deleteCount`가 0 이하라면 어떤 요소도 제거하지 않습니다. 이 때는 최소한 하나의 새로운 요소를 지정해야 합니다.
- `item1, item2, ...`(Optional) : 배열에 추가할 요소입니다. 아무 요소도 지정하지 않으면 `splice()`는 요소를 제거하기만 합니다.

### 반환 값

제거한 요소를 담은 배열. 하나의 요소만 제거한 경우 길이가 1인 배열을 반환합니다. 아무 값도 제거하지 않았으면 빈 배열을 반환합니다.

## 설명

만약 제거할 요소의 수와 추가할 요소의 수가 다른 경우 배열의 길이는 달라집니다.

## 레퍼런스

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/splice