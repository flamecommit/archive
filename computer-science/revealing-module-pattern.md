# 노출모듈 패턴 (revealing-module-pattern)

노출모듈 패턴은 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴을 말합니다. 자바스크립트는 private나 public 같은 접근 제어자가 존재하지 않고 전역 범위에서 스크립트가 실행됩니다. 그렇기 때문에 노출모듈 패턴을 통해 private와 public 접근 제어자를 구현하기도 합니다.

```js
const pukuba = (() => {
  const a = 1;
  const b = () => 2;
  const public = {
    c: 2,
    d: () => 3
  }
  return public;
})();
console.log(pukuba);
console.log(pukuba.a);
// { c: 2, d: [Function: d] }
// undefined
```

a와 b는 다른 모듈에서 사용할 수 없는 변수나 함수이며 private 범위를 가집니다. c와 d는 다른 모듈에서 사용할 수 있는 변수나 함수이며 public 범위를 가집니다. 참고로 앞서 설명한 노출모듈 패턴을 기반으로 만든 자바스크립트 모듈 방식으로는 CJS(CommonJS) 모듈 방식이 있습니다.
