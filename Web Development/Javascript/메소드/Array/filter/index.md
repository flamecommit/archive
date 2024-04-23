# Array.prototype.filter()

- 메서드 분류 : 순회 메서드
- 원본 수정 : X
- 반환 타입 : Array
- 복사 종류 : 얕은 복사

`Array` 인스턴스의 `filter()` 메서드는 주어진 배열의 일부에 대한 얕은 복사본을 생성하고, 주어진 배열에서 제공된 함수에 의해 구현된 테스트를 통과한 요소로만 필더링 합니다.

```js
const words = ['spray', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter((word) => word.length > 6);

console.log(words);
// ['spray', 'elite', 'exuberant', 'destruction', 'present']
console.log(result);
// ["exuberant", "destruction", "present"]
```