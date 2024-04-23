# Array.prototype.filter()

- 설명 : 배열을 순회하며 함수에 의해 구현된 테스트를 통과한 요소만 필터링 하는 메소드.
- 메서드 분류 : 순회 메서드
- 원본 수정 : X
- 반환 타입 : Array
- 복사 종류 : 얕은 복사

```js
const words = ['spray', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter((word) => word.length > 6);

console.log(words);
// ['spray', 'elite', 'exuberant', 'destruction', 'present']
console.log(result);
// ["exuberant", "destruction", "present"]
```