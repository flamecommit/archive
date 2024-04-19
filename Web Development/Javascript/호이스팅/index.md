# 호이스팅(Hoisting) 이란?

호이스팅이란 "끌어올린다" 라는 뜻으로 변수 및 함수 선언문이 스코프 내의 최상단으로 끌어올려지는 현상을 말합니다. 여기서 주의할 점은 "선언문" 이라는 것이며 "대입문"은 끌어올려지지 않습니다.

> **모범 답변**
>
> 실행 컨텍스트 생성 시 렉시컬 스코프 내의 선언이 끌어올려 지는게 호이스팅입니다.

## 선언하기 전 사용할 수 있는 var

`var` 선언은 함수가 시작될 때 처리됩니다. 전역에서 선언한 변수라면 스크립트가 시작될 때 처리되죠.

함수 본문 내에서 `var`로 선언한 변수는 선언 위치와 상관없이 함수 본문이 시작되는 지점에서 정의됩니다(단, 변수가 중첩 함수 내에서 정의되지 않아야 이 규칙이 적용됩니다).

따라서 아래 두 예제는 동일하게 동작합니다.

```js
function sayHi() {
    phrase = "Hello";

    alert(phrase);

    var phrase;
}

sayHi();
```

`var phrase`가 위로 이동한 것처럼 말이죠.

```js
function sayHi() {
  var phrase;

  phrase = "Hello";

  alert(phrase);
}
sayHi();
```

코드 블록은 무시되기 때문에, 아래 코드 역시 동일하게 동작합니다.

```js
function sayHi() {
  phrase = "Hello"; // (*)

  if (false) {
    var phrase;
  }

  alert(phrase);
}
sayHi();
```

이렇게 변수가 끌어올려 지는 현상을 '호이스팅(hoisting)'이라고 부릅니다. `var`로 선언한 모든 변수는 함수의 최상위로 '끌어 올려지기(hoisted)' 때문입니다.

바로 위 예제에서 `if (false)` 블록 안 코드는 절대 실행되지 않지만, 이는 호이스팅에 전혀 영향을 주지 않습니다. `if` 내부의 `var` 는 함수 `sayHi` 의 시작 부분으로 처리되므로 `(*)` 로 표시한 줄에서 `phrase` 는 이미 정의가 된 상태인 것이죠.

**선언은 호이스팅 되지만 할당은 호이스팅 되지 않습니다.**

예시를 통해 살펴봅시다.

```js
function sayHi() {
  alert(phrase);

  var phrase = "Hello";
}

sayHi();
```

`var phrase = "Hello"` 행에선 두 가지 일이 일어납니다.

1. 변수 선언(`var`)
2. 변수에 값을 할당(`=`)

변수 선언은 함수 실행이 시작될 때 처리되지만(호이스팅) 할당은 호이스팅 되지 않기 때문에 할당 관련 코드에서 처리됩니다. 따라서 위 예제는 아래 코드처럼 동작하게 됩니다.

```js
function sayHi() {
  var phrase; // 선언은 함수 시작 시 처리됩니다.

  alert(phrase); // undefined

  phrase = "Hello"; // 할당은 실행 흐름이 해당 코드에 도달했을 때 처리됩니다.
}

sayHi();
```

이처럼 모든 `var` 선언은 함수 시작 시 처리되기 때문에, `var`로 선언한 변수는 어디서든 참조할 수 있습니다. 하지만 변수에 무언가를 할당하기 전까진 값이 undefined이죠.

바로 위의 두 예시에서 `alert` 를 호출하기 전에 변수 `phrase` 는 선언이 끝난 상태이기 때문에 에러 없이 얼럿 창이 뜹니다.

## 레퍼런스
- https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42
- https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/hoisting.md#gear-%EC%9D%B8%ED%84%B0%ED%94%84%EB%A6%AC%ED%8C%85
- https://ko.javascript.info/var