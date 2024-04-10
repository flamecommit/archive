# 옵저버 패턴

옵저버 패턴은 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴입니다.

여기서 주체랑 객체의 상태 변화를 보고 있는 관찰자이며, 옵저버들이란 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 '추가 변화 사항'이 생기는 객체들을 의미합니다.

또한, 앞의 그림처럼 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 합니다.

옵저버 패턴을 활용한 서비스로는 트위터가 있습니다.

내가 어떤 사람인 주체를 '팔로우'했다면 주체가 포스팅을 올릴때 '팔로워'에게 알람이 가게 됩니다.

또한, 옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC 패턴에도 사용됩니다.

예를 들어 주체라고 볼 수 있는 모델에서 변경 사항이 생겨 update() 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러 등이 작동합니다.

## 자바스크립트에서의 옵저버 패턴

자바스크립트에서의 옵저버 패턴은 프록시 객체를 통해 구현할 수 있습니다.

### 프록시 객체

프록시(proxy) 객체는 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체를 뜻하며, 자바스크립트에서 프록시 객체는 두 개의 매개변수를 가집니다.

- target: 프록시할 대상
- handler: target 동작을 가로채고 어떠한 동작을 할 것인지가 설정되어 있는 함수

다음은 프록시 객체를 구현한 코드입니다.

```js
const handler = {
  get: function(target, name) {
    return name === 'name' ? `${target.a} ${target.b}` : target[name];
  }
}
const p = new Proxy({ a: 'KUNDOL', b: 'IS AUMUMU ZANGIN' }, handler);
console.log(p.name); // KUNDOL IS AUMUMU ZANGIN
```

new Proxy()로 a와 b속성을 가지고 있는 객체와 handler 함수를 매개변수로 넣고 p라는 변수를 선언했습니다. 이후 p의 name 속성을 참조하니 a와 b라는 속성밖에 없는 객체가 handler의 "name이라는 속성에 접근할 때 a와 b를 합쳐서 문자열을 만들어라"는 로직에 따라 어떤 문자열을 만듭니다. 이렇게 name 속성 등 특정 속성에 접근할 때 그 부분을 가로채서 어떠한 로직을 강제할 수 있는 것이 프록시 객체입니다.

### 프록시 객체를 이용한 옵저버 패턴

그렇다면 자바스크립트의 프록시 객체를 통해 옵저버 패턴을 구현해보겠습니다.

```js
function createReactiveObject(target, callback) {
  const proxy = new Proxy(target, {
    set(obj, prop, value) {
      if (value !== obj[prop]) {
        const prev = obj[prop];
        obj[prop] = value;
        callback(`${prop}가 [${prev}] >> [${value}] 로 변경되었습니다.`);
        return true;
      }
    }
  });
  return proxy;
}

const a = {
  "형규": "솔로"
};

const b = createReactiveObject(a, console.log);

b.형규 = "솔로";
b.형규 = "커플";
```

- get() : 속성에 대한 읽기를 가로챕니다.
- has() : in 연산자의 사용을 가로챕니다.
- set() : 속성에 대한 쓰기를 가로챕니다.

## Vue.js 3.0의 옵저버 패턴

프론트엔드에서 많이 쓰는 프레임워크 Vue.js 3.0에서 ref나 reactive로 정의하면 해당 값이 변경되었을 때 자동으로 DOM에 있는 값이 변경되는데, 이는 앞서 설명한 프록시 객체를 이용한 옵저버 패턴을 이용하여 구현한 것입니다.

*DOM(Document Object Model)*
> 문서 객체 모델을 말하며, 웹 브라우저상의 화면을 이루고 있는 요소들을 지칭합니다.







