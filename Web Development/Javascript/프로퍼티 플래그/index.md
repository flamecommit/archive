# 프로퍼티 플래그

아시다시피 객체엔 프로퍼티가 저장됩니다.

지금까진 프로퍼티를 단순히 '키-값' 쌍의 관점에서만 다뤘습니다. 그런데 사실 프로퍼티는 우리가 생각했던 것보다 더 유연하고 강력한 자료구조입니다.

## 프로퍼티 플래그

객체 프로퍼티는 `값(value)` 과 함께 플래그(flag)라 불리는 특별한 속성 세 가지를 갖습니다.

- `writable` - `true` 이면 값을 수정할 수 있습니다. 그렇지 않다면 읽기만 가능합니다.
- `enumerable` - `true` 이면 반복문을 사용해 나열할 수 있습니다. 그렇지 않다면 반복문을 사용해 나열할 수 없습니다.
- `configurable` - `ture` 이면 프로퍼티 삭제나 플래그 수정이 가능합니다. 그렇지 않다면 프로퍼티 삭제와 플래그 수정이 불가능합니다.

지금까지 해왔던 '평범한 방식'으로 프로퍼티를 만들면 해당 프로퍼티의 플래그는 모두 `true`가 됩니다. 이렇게 `true`로 설정된 플래그는 언제든 수정할 수 있습니다.

Object.getOwnPropertyDescriptor메서드를 사용하면 특정 프로퍼티에 대한 정보를 모두 얻을 수 있습니다.

문법:

```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

- `obj` - 정보를 얻고자 하는 객체
- `propertyName` - 정보를 얻고자 하는 객체 내 프로퍼티

메서드를 호출하면 "프로퍼티 설명자(descriptor)"라고 불리는 객체가 반환되는데, 여기에는 프로퍼티 값과 세 플래그에 대한 정보가 모두 담겨있습니다.

예시:

```js
let user = {
    name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, "name");

alert(JSON.stringify(descriptor, null, 2));

/* property descriptor:
{
    "value": "John",
    "writable": true,
    "enumerable": true,
    "configurable": true
}
*/
```

메서드 Object.defineProperty를 사용하면 플래그를 변경할 수 있습니다.

문법:

```js
Object.defineProperty(obj, propertyName, descriptor);
```

- `obj`, `propertyName` - 설명자를 적용하고 싶은 객체와 객체 프로퍼티
- `descriptor` - 적용하고자 하는 프로퍼티 설명자

`defineProperty` 메서드는 객체에 해당 프로퍼티가 있으면 플래그를 원하는 대로 변경해줍니다. 프로퍼티가 없으면 인수로 넘겨받은 정보를 이용해 새로운 프로퍼티를 만듭니다. 이때 플래그 정보가 없으면 플래그 값은 자동으로 `false`가 됩니다.

아래 예시를 보면 프로퍼티 `name`이 새로 만들어지고, 모든 플래그 값이 `false`가 된 것을 확인할 수 있습니다.

```js
let user = {};

Object.defineProperty(user, "name", {
    value: "John"
});

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "John",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
 */
```

'평범한 방식으로' 객체 프로퍼티 `user.name`을 만들었을 때와 `defineProperty`를 이용해 프로퍼티를 만들었을때의 가장 큰 차이점은 플래그에 있습니다. `defineProperty`를 사용해 프로퍼티를 만든 경우, `descriptor`에 플래그 값을 명시하지 않으면 플래그 값이 자동으로 `false`가 됩니다. 플래그 값을 `true`로 설정하려면 `descriptor`에 `true`라고 명시해 주어야 합니다. 이제 예시를 통해 플래그의 효과에 대해 알아봅시다.

## writable 플래그

`writable` 플래그를 사용해 `user.name`에 값을 쓰지 못하도록(non-writable) 해봅시다.

```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false
});

user.name = "Pete"; // Error: Cannot assign to read only property 'name'
```

이제 `defineProperty`를 사용해 `writable` 플래그를 `true` 로 변경하지 않는 한 그 누구도 객체의 이름을 변경할 수 없게 되었습니다.


> **에러는 엄격 모드에서만 발생합니다.**
>
> 비 엄격 모드에선 읽기 전용 프로퍼티에 값을 쓰려고 해도 에러가 발생하지 않습니다. 다만 이때 값을 변경하는 것은 불가능합니다. 비 엄격 모드에선 이와 같이 플래그에서 정한 규칙을 위반하는 행위는 에러 없이 그냥 무시됩니다.

## enumerable

`user`에 커스텀 메서드 `toString`을 추가해봅시다.

객체 내장 메서드 `toString`은 열거가 불가능(non-enumerable)하기 때문에 `for..in`사용시 나타나지 않습니다. 하지만 커스텀 `toString`을 추가하면 아래와 같이 `for..in`에 `toString`이 나타납니다.

```js
let user = {
    name: "John",
    toString() {
        return this.name;
    }
};

// 커스텀 toString은 for...in을 사용해 열거할 수 있습니다.
for (let key in user) alert(key); // name, toString
```

그런데 특정 프로퍼티의 `enumerable` 플래그 값을 `false`로 설정하면 커스텀 `toString`도 열거가 불가능하게 할 수 있습니다.

```js
let user = {
    name: "John",
    toString() {
        return this.name;
    }
};

Object.defineProperty(user, "toString", {
    enumerable: false
});

for (let key in user) alert(key); // name
```

열거가 불가능한 프로퍼티는 `Object.keys`에도 배제됩니다.

```js
alert(Object.keys(user)); // name
```

## configurable 플래그

구성 가능하지 않음을 나타내는 플래그(non-configurable flag)인 `configurable: false`는 몇몇 내장 객체나 프로퍼티에 기본으로 설정되어 있습니다.

어떤 프로퍼티의 `configurable` 플래그가 `false` 로 설정되어 있다면 해당 프로퍼티는 객체에서 지울 수 없습니다.

내장 객체 `Math`의 `PI` 프로퍼티가 대표적인 예입니다. 이 프로퍼티는 쓰기와 열거, 구성이 불가능합니다.

```js
let descriptor = Object.getOwnPropertyDescriptor(Math, 'PI');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": 3.141592653589793,
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```

개발자가 코드를 사용해 `Math.PI` 값을 변경하거나 덮어쓰는 것도 불가능합니다.

```js
Math.PI = 3; // Error
```

`configurable` 플래그를 `false`로 설정하면 돌이킬 방법이 없습니다. `defineProperty`를 써도 값을 `true`로 되돌릴 수 없죠.

`configurable: false`가 만들어내는 구체적인 제약사항은 아래와 같습니다.

1. `configurable` 플래그를 수정할 수 없음
2. `enumerable` 플래그를 수정할 수 없음.
3. `writable: false`의 값을 `true`로 바꿀 수 없음(`true`를 `false`로 변경하는 것은 가능함).
4. 접근자 프로퍼티 `get/set`을 변경할 수 없음(새롭게 만드는 것은 가능함).

이런 특징을 이용하면 아래와 같이 "영원히 변경할 수 없는" 프로퍼티(`user.name`)를 만들 수 있습니다.

```js
let user = { };

Object.defineProperty(user, "name", {
  value: "John",
  writable: false,
  configurable: false
});

// user.name 프로퍼티의 값이나 플래그를 변경할 수 없습니다.
// 아래와 같이 변경하려고 하면 에러가 발생합니다.
//   user.name = "Pete"
//   delete user.name
//   Object.defineProperty(user, "name", { value: "Pete" })
Object.defineProperty(user, "name", {writable: true}); // Error
```

> **"non-configurable"은 "non-writable"과 다릅니다.**
>
> `configurable` 플래그가 `false` 이더라도 `writable` 플래그가 `true` 이면 프로퍼티 값을 변경할 수 있습니다.
> `configurable: false`는 플래그 값 변경이나 프로퍼티 삭제를 막기 위해 만들어졌지, 프로퍼티 값 변경을 막기 위해 만들어진 게 아닙니다.


## 레퍼런스
- https://ko.javascript.info/property-descriptors