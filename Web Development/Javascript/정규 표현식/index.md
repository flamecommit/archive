# 정규 표현식

정규 표현식, 또는 정규식은 문자열에서 특정 문자 조합을 찾기 위한 패턴입니다. JavaScript에서는 정규 표현식도 객체로서, `RegExp`의 `exec()`와 `test()` 메서드를 사용할 수 있습니다. `String`의 `match()`, `matchAll()`, `replace()`, `replaceAll()`, `search()`, `split()` 메서드와도 함께 사용할 수 있습니다. 이 장에서는 JavaScript의 정규 표현식을 설명합니다.

## 정규 표현식 만들기

정규 표현식은 두 가지 방법으로 만들 수 있습니다.

- 정규 표현식 리터럴. 다음과 같이 슬래시로 패턴을 감싸서 작성합니다.

```js
const re = /ab+c/;
```

정규 표현식 리터럴은 스크립트를 불러올 때 컴파일되므로, 파뀔 일이 없는 패턴의 경우 리터럴을 사용하면 성능이 향상될 수 있습니다.

- `RegExp` 객체의 생성자 호출.

```js
const re = new RegExp("ab+c");
```

생성자 함수를 사용하면 정규 표현식이 런타임에 컴파일됩니다. 바뀔 수 있는 패턴이나, 사용자 입력 등 외부 출처에서 가져오는 패턴의 경우 이렇게 사용하세요.

## 정규 표현식 패턴 작성하기

정규 표현식 패턴은 `/abc/`처럼 단순한 문자로 구성하거나, `/ab+c/`와 `/Chapter (\d+)\.\d*/`처럼 단순한 문자와 특수 문자의 조합으로 구성할 수도 있습니다. 특히 `(\d+)`에 나타난 괄호는 정규 표현식에서 기억 장치처럼 쓰여서, 괄호의 안쪽 패턴과 일치한 부분을 나중에 사용할 수 있도록 기억합니다.

### 단순 패턴 사용하기

단순 패턴은 문자열을 있는 그대로 탐색할 때 사용합니다. 예를 들어, `/abc/` 패턴은 문자열에서 정확한 순서로 `"abc"`라는 문자의 조합이 나타나는 부분과 일치합니다. 그러므로 이 패턴은 `"Hi, do you know your abc's?"`와 `"The latest airplane designs evolved from slabcraft."` 두 문자열에서 일치에 성공하고, 일치하는 부분은 `"abc"`일 것입니다. 반면 `"Grab crab"`에서는 일치하지 않는데, 이 문자열은 부분 문자열로 `"abc"`를 포함하긴 하지만, 정확하게 `"abc"`를 포함하지는 않기 때문입니다.

### 특수 문자 사용하기

하나 이상의 "b"를 찾는다거나 공백 문자를 찾는 등 직접적인 일치 이상의 탐색이 필요할 땐 특수 문자를 사용합니다. 예컨데 "하나는 `"a"` 이후에 0개 이상의 `"b"`, 그 뒤의 `"c"`와 일치해야 하면 `/ab*c/` 패턴을 사용할 수 있습니다. `"b"` 뒤의 `*`는 "이전 항목의 0번 이상 반복"을 의미합니다. 이 패턴을 문자열 `"cbbabbbbcdebc"`에 대해 사용하면, 일치하는 부분 문자열은 `"abbbbc"`일 것입니다.

아래의 문서들에선 각각의 범주에 속하는 다양한 특수 문자의 목록과 설명, 예제를 찾아볼 수 있습니다.

- 어서션 - 어서션에는 줄이나 단어의 시작과 끝을 나타내는 경계와, 일치가 가능한 방법을 나타내는 패턴(전방탐색, 후방탐색, 조건 표현식 등)이 포함됩니다.
- 문자 클래스 - 글자와 숫자처럼 다른 유형의 문자를 구분합니다.
- 그룹과 범위 - 표현 문자의 그룹과 범위를 나타냅니다.
- 수량자 - 일치할 문자나 표현이 반복되어야 할 횟수를 나타냅니다.
- 유니코드 속성 이스케이프 - 대/소문자, 수학 기호, 문장 부호처럼, 유니코드 문자 속성에 따라 문자를 구분합니다.

### 이스케이핑

특수 문자를 있는 그대로 탐색(`"*"`을 직접 찾는 등)해야 하는 경우, 특수 문자 앞에 역슬래시(\\)를 배치해서 이스케이프 해야 합니다. 예를 들어 `"a"`뒤의 별표(`"*"`) 뒤의 `"b"`와 일치해야 하면 `"a\*b/`를 사용하면 됩니다. 역슬래시가 `"*"`를 "이스케이프"해서, 특수 문자가 아닌 문자 리터럴로 취급합니다.

마찬가지로, 슬래시(/)와 일치해야 하는 경우에도 이스케이프를 해야 합니다. 그냥 빗금을 사용하면 패턴이 끝나버립니다. 예를들어 문자열 "/example/"과 그 뒤 하나 이상의 알파벳을 찾으려면 `/\/example\/[a-z]/`를 사용할 수 있습니다. 각각의 슬래시 앞에 놓인 역슬래시가 슬래시를 이스케이프합니다.

리터럴 역슬래시에 일치하려면 역슬래시를 이스케이프합니다. "A:\", "B:\", "C:\", ..., "Z:\"와 일치하는 패턴은 `/[A-Z]:\\/`입니다. 앞의 역슬래시가 뒤의 역슬래시를 이스케이프해서, 결과적으로 하나의 리터럴 역슬래시와 일치하게 됩니다.

`RegExp` 생성자와 문자열 리터럴을 사용하는 경우, 역슬래시가 문자열 리터럴의 이스케이프로도 작동한다는 것을 기억해야 합니다. 그러므로 정규 표현식의 역슬래시를 나타내려면 문자열 리터럴 수준의 이스케이프도 해줘야 합니다. 즉, 앞서 살펴본 `/a\*b/` 패턴을 생성하려면 `new RegExp("a\\*b")`가 되어야 합니다.

이스케이프 되지 않은 문자열을 이미 가지고 있을 땐 `String.replace`를 활용해 이스케이프를 해줄 수 있습니다.

```js
function escapeRegExp(string) {
    return string.replace(/[.*+?^${}()|[\]\\]/g,"\\$&"); // $&은 일치한 문자열 전체를 의미
}
```

정규 표현식 뒤의 "g"는 전체 문자열을 탐색해서 모든 일치를 반환하도록 지정하는 전역 탐색 플래그입니다.

### 괄호 사용하기

정규 표현식의 아무 부분이나 괄호로 감싸게 되면, 그 부분과 일치하는 부분 문자열을 기억하게 됩니다. 기억한 부분 문자열은 불러와서 다시 사용할 수 있습니다.

## JavaScript에서 정규 표현식 사용하기

정규 표현식은 `RegExp`의 메서드 `test()`와 `exec()`, `String`의 메서드 `match()`, `replace()`, `search()`, `split()`에서 사용할 수 있습니다.

|---|---|
|메서드|설명|
|---|---|
|`exec()`|문자열에서 일치하는 부분을 탐색합니다. 일치 정보를 나타내는 배열, 또는 일치가 없는 경우 `null`을 반환합니다.|
|`test()`|문자열에 일치하는 부분이 있는지 확인합니다. `true`또는 `false`를 반환합니다.|
|`match()`|캡처 그룹을 포함해서 모든 일치를 담은 배열을 반환합니다. 일치가 없으면 `null`을 반환합니다.|
|`matchAll()`|캡처 그룹을 포함해서 모든 일치를 담은 반복기를 반환합니다.|
|`search()`|문자열에서 일치하는 부분을 탐색합니다. 일치하는 부분의 인덱스, 또는 일치가 없는 경우 `-1`을 반환합니다.|
|`replace()`|문자열에서 일치하는 부분을 탐색하고, 그 부분을 대체 문자열로 바꿉니다.|
|`replaceAll()`|문자열에서 일치하는 부분을 모두 탐색하고, 모두 대체 문자열로 바꿉니다.|
|`split()`|정규 표현식 또는 문자열 리터럴을 사용해서 문자열을 부분 문자열의 배열로 나눕니다.|

문자열 내부에 패턴과 일치하는 부분이 존재하는지만 알아내려면 `test()`나 `search()` 메서드를 사용하세요. 더 느리더라도 일치에 관한 추가 정보가 필요하면 `exec()`과 `match()` 메서드를 사용하세요. 일치하는 부분이 존재하면, `exec()`과 `match()`는 일치에 관한 데이터를 포함한 배열을 반환하고, 일치에 사용한 정규 표현식 객체의 속성을 업데이트합니다. 일치하지 못한 경우 `null`을 반환합니다. (`null`은 조건 평가 시 `false`와 같습니다)

아래의 예제에서는, 문자열에서 일치하는 부분을 찾기 위해 `exec()` 메서드를 사용합니다.

```js
const myRe = /d(b+)d/g;
const myArray = myRe.exec("cdbbdbsbz")
```

만약 정규 표현식 객체의 속성에 접근할 필요가 없으면 아래와 같이 짧게 쓸 수도 있습니다.

```js
const myArray = /d(b+)d/g.exec("cdbbdbsbz");
// 'cdbbdbsbz'.match(/d(b+)d/g); 와 비슷하지만,
// 'cdbbdbsbz'.match(/d(b+)d/g)의 반환 값은 [ 'dbbd' ]인 반면
// /d(b+)d/g.exec('cdbbdbsbz')의 반환 값은 [ 'dbbd', 'bb', index: 1, input: 'cdbbdbsbz' ]
```

정규 표현식을 문자열에서 만들고 싶으면 아래처럼 사용할 수도 있습니다.

```js
const myRe = new RegExp("d(b+)d", "g");
const myArray = myRe.exec("cdbbdbsbz");
```

아래의 표는 위 스크립트에서 일치를 성공한 후, 반환하는 배열과 업데이트되는 정규 표현식 객체의 속성입니다.

<table>
  <caption>정규 표현식 실행 결과</caption>
  <thead>
    <tr>
      <th scope="col">객체</th>
      <th scope="col">속성 또는 인덱스</th>
      <th scope="col">설명</th>
      <th scope="col">위 예제에서의 값</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="4"><code>myArray</code></td>
      <td></td>
      <td>일치한 문자열 및 기억한 모든 부분 문자열.</td>
      <td><code>['dbbd', 'bb', index: 1, input: 'cdbbdbsbz']</code></td>
    </tr>
    <tr>
      <td><code>index</code></td>
      <td>일치한 부분이 주어진 문자열에서 위치한 인덱스. (0부터 시작)</td>
      <td><code>1</code></td>
    </tr>
    <tr>
      <td><code>input</code></td>
      <td>주어진 원본 문자열.</td>
      <td><code>'cdbbdbsbz'</code></td>
    </tr>
    <tr>
      <td><code>[0]</code></td>
      <td>마지막으로 일치한 부분 문자열.</td>
      <td><code>'dbbd'</code></td>
    </tr>
    <tr>
      <td rowspan="2"><code>myRe</code></td>
      <td><code>lastIndex</code></td>
      <td>
        다음 일치를 시작할 인덱스. (g 옵션을 지정한 정규 표현식의 경우에만 설정됩니다. 플래그를 활용한 고급 탐색을 참고하세요)
      </td>
      <td><code>5</code></td>
    </tr>
    <tr>
      <td><code>source</code></td>
      <td>패턴의 텍스트. 정규 표현식이 생성될 때 갱신됩니다. 실행 시점에는 갱신되지 않습니다.</td>
      <td><code>'d(b+)d'</code></td>
    </tr>
  </tbody>
</table>

위 예제의 두 번째 형태처럼, 정규 표현식 객체를 변수에 대입하지 않고도 사용할 수 있습니다. 하지만, 이러면 매 사용마다 정규 표현식 객체가 새로 생성되며, 업데이트되는 속성에 접근할 수 없습니다. 다음과 같은 코드를 생각해보겠습니다.

```js
const myRe = /d(b+)d/g;
const myArray = myRe.exec("cdbbdbsbz");
console.log(`lastIndex의 값은 ${myRe.lastIndex}`);

// "lastIndex의 값은 5"
```

그러나 위의 코드 대신 아래 코드를 사용하게 되면

```js
const myArray = /d(b+)d/g.exec("cdbbdbsbz");
console.log(`lastIndex의 값은 ${/d(b+)d/g.lastIndex}`);

// "lastIndex의 값은 0"
```

두 개의 `/d(b+)d/g`는 서로 다른 정규 표현식 객체이므로 별개의 `lastIndex` 속성을 갖습니다. 정규 표현식 객체의 속성에 접근해야 하면, 우선 변수에 할당하세요.


### 플래그를 활용한 고급 탐색

정규 표현식은 전역 탐색이나 대소문자 무시와 같은 특성을 지정하는 플래그를 가질 수 있습니다. 플래그는 단독으로 사용할 수도 있고, 순서에 상관 없이 한꺼번에 여럿을 지정할 수도 있습니다.

|---|---|
|플래그|설명|
|---|---|
|`d`|부분 문자열 일치에 대해 인덱스 생성|
|`g`|전역 탐색|
|`i`|대소문자를 구분하지 않음|
|`m`|여러 줄에 걸쳐 탐색|
|`s`|개행 문자가 `.`과 일치함|
|`u`|"unicode", 패턴을 유니코드 코드 포인트의 시퀀스로 간주함|
|`y`|"접착" 탐색, 대상 문자열의 현재 위치에서 탐색을 시작함|

플래그는 다음과 같은 구문으로 정규 표현식에 지정할 수 있습니다.

```js
const re = /pattern/flags;
```

생성자를 사용할 경우 이렇게 지정합니다.

```js
const re = new RegExp("pattern", "flags");
```

플래그는 정규식과 완전히 합쳐지므로 나중에 추가하거나 제거할 수 없습니다.

예를 들어, `re = /\w+\s/g`는 한 개 이상의 글자와 그 뒤의 공백 하나를, 문자열 전체에 대해 탐색합니다.

```js
const re = /\w+\s/g;
const str = "fee fi fo fum";
const myArray = str.match(re);
console.log(myArray);

// ["fee ", "fi ", "fo "]
```

아래 코드는...

```js
const re = /\w+\s/g;
```

이렇게 생성자를 사용하도록 바꿀 수도 있습니다.

```js
const re = new RegExp("\\w+\\s", "g");
```

두 구문 모두 동일한 결과를 낳습니다.

`m` 플래그는 여러 줄에 걸친 입력 문자열을 여러 줄로 취급하게 합니다. 달리 말해, `m`플래그를 지정할 경우, `^`와 `$`는 각각 전체 입력 문자열의 시작과 끝이 아니라, 각 줄의 시작과 끝에 대응하게 됩니다.

### exec()과 전역 탐색 플래그 사용하기

`RegExp.prototype.exec()` 메서드와 `g` 플래그를 사용하면, 일치한 부분 문자열들과 각각의 인덱스를 하나씩 순차적으로 반환합니다.

```js
const str = "fee fi fo fum";
const re = /\w+\s/g;

console.log(re.exec(str)); // ["fee ", index: 0, input: "fee fi fo fum"]
console.log(re.exec(str)); // ["fi ", index: 4, input: "fee fi fo fum"]
console.log(re.exec(str)); // ["fo ", index: 7, input: "fee fi fo fum"]
console.log(re.exec(str)); // null
```

반면, `String.prototype.match()` 메서드는 모든 일치를 한 번에 반환하지만, 각각의 인덱스는 포함하지 않습니다.

```js
console.log(str.match(re)); // ["fee ", "fi ", "fo "]
```

## 정규식 기호 모음

### 정규식 특정 문자 숫자 매칭 패턴

|---|---|
|패턴|의미|
|---|---|
|a-zA-Z|영어알파벳(-으로 범위 지정)|
|ㄱ-ㅎ가-힣|한글 문자(-으로 범위 지정)|
|0-9|숫자(-으로 범위 지정)|
|.|모든 문자열(숫자, 한글, 영어, 특수기호, 공백 모두)<br />단, 줄바꿈 X|
|\d|숫자|
|\D|숫자가 아닌 것|
|\w|밑줄 문자를 포함한 영숫자 문자에 대응<br />[A-Za-z0-9_]와 동일|
|\W|\w가 아닌 것|
|\s|space 공백|
|\S|space 공백이 아닌 것|
|\특수기호|특수기호 \\*\^\& 등|
|\b|63개 문자(영문 대소문자 52개 + 숫자 10개 + _(underscore))가 아닌 나머지 문자에 일치하는 경계(boundary)|
|\B|63개 문자에 일치하는 경계|
|\x|16진수 문자에 일치<br />/\x61/는 a에 일치|
|\0|8진수 문자에 일치<br />/\141/은 a에 일치|
|\u|유니코드(Unicode) 문자에 일치<br />/\u0061/는 a에 일치|
|\c|제어(Control) 문자에 일치|
|\f|폼 피드(FF, U+000C) 문자에 일치|
|\n|줄 바꿈(LF, U+000D) 문자에 일치|
|\r|캐리지 리턴(CR, U+000D) 문자에 일치|
|\t|탭 (U+0009) 문자에 일치|

### 정규식 검색 기준 패턴

|---|---|
|기호|의미|
|---|---|
|`\|`|OR<br /> `a\|b`|
|`[]`|괄호안의 문자들 중 하나. `or 처리 묶음`으로 보면 된다.<br />`/abc/`: "abc"를 포함하는<br />`/[abc]/`: "a"또는 "b"또는 "c"를 포함하는<br />`[다-바]`: 다 or 라 or 마 or 바|
|`[^문자]`|괄호안의 문자를 제외한 것<br />`[^lgEn]`: "l", "g", "E", "n" 4개 문자를 제외<br />※ 대괄호 안에서 사용하면 제외의 뜻, 대괄호 밖에서 쓰면 시작점의 뜻|
|`^문자열`|특정 문자열로 시작 (시작점)<br />`/^http/`|
|`문자열$`|특정 문자열로 끝남 (종착점)<br />`/^com/`|

### 정규식 갯수 반복 패턴

|---|---|
|기호|의미|
|---|---|
|`?`|없거나 or 최대 한개만<br />`/apple?/`<br />`/apple?/.exec("appl")`: `appl`|
|`*`|없거나 or 있거나 (여러개)<br />`/apple*/`<br />`/apple*/.exec("appleeeeeeeeeeee")`: `appleeeeeeeeeeee`|
|`+`|최소 한개 or 여러개<br />`/apple+/`|
|`*?`|없거나, 있거나 and 없거나, 최대한개 : 없음<br />`{0}`과 동일|
|`+?`|최소한개, 있거나 and 없거나, 최대한개 : 한개<br />`{1}`과 동일|
|`{n}`|n개|
|`{Min,}`|최소 Min개 이상|
|`{Min, Max}`|최소 Min개 이상, 최대 Max개 이하<br />{3,5}?=={3}과 동일|

### 정규식 그룹 패턴

|---|---|
|기호|의미|
|---|---|
|`()`|그룹화 및 캡쳐|
|`(?:패턴)`|그룹화 (캡쳐 X)|
|`(?=)`|앞쪽 일치(Lookahead)|
|`(?!)`|부정 앞쪽 일치(Negative Lookahead)|
|`(?<=)`|뒤쪽 일치(Lookbehind)|
|`(?<!)`|부정 뒤쪽 일치(Negative Lookbehind)|

#### 정규식 그룹화

```js
'kokokoko'.match(/ko+/); // "ko"
'kooookoooo'.match(/ko+/); // "koooo"
```

코드를 보면 알 수 있듯이, 표현식 `ko+`는 `"o"`만 `+`를 적용시킵니다.. ("k"는 적용 안 됨) 그 결과로 "koooo"가 반환되었습니다..

```js
'kokokoko'.match(/(ko)+/); // "kokokoko", "ko"
'kooookoooo'.match(/(ko)+/); // "ko", "ko"
```

하지만 표현식 `(ko)+`는 `"k"`와 `"o"`를 그룹화 했기 때문에 `"ko"`를 검색하게 됩니다. 

그런데 패턴`()`을 사용한 정규식들의 결과가 `g`플래그를 사용하지 않았는데도 2개씩 나왔습니다. 이것은 정규식 그룹화의 캡처 기능때문에 그렇습니다.

#### 정규식 캡처 기능

패턴 그룹화 `()`는 괄호 안에 있는 표현식을 캡처하여 사용합니다.

캡처는 일종의 복사본을 생성하는 개념이라고 보면 됩니다.

```js
'kokokoko'.match(/(ko)+/); // "kokokoko", "ko"
```

정규식의 캡처 원리는 알아보면, 패턴 ()안에 있는 "ko"를 그룹화하여 캡처합니다.

우선 캡처된 표현식은 당장 사용되지 않으며, 그룹화된 "ko"를 패턴+로 1회 이상 연속 반복되는 문자를 검색합니다. 그렇게 캡처 외 표현식이 모두 작동하고 난 뒤에 캡처된 표현식 "ko"가 검색되는 것입니다.

즉, 위의 검색 순서를 정리하면 다음과 같습니다.

1. 그룹화된 "ko"를 패턴 +로 1회 이상 연속으로 반복하여 검색 ("kokokoko" 반환)
2. 캡처된 "ko"로 검색하여 "ko"를 추가 반환

```js
'123abc'.match(/(\d+)(\w)/); // "123a", "123", "a"
/*
1. 패턴 ()안의 표현식을 순서대로 캡처. \d+, \w
2. 캡처 후 남은 표현식으로 검색.
3. 패턴 \d로 숫자를 검색하되 패턴 +로 1개 이상 연속되는 숫자를 검색 → "123"
4. 다음 패턴 \w는 문자를 검색하니 "a"가 일치 
5. 최종적으로 "123a"가 반환.

6. 첫 번째 캡처한 표현식 \d+로 숫자를 재검색하되 패턴 +로 1개 이상 연속되는 숫자를 검색
7. "123"가 일치하여 반환

8. 나머지 캡처한 표현식 \w로 문자를 검색하니 "a"가 일치하여 반환
*/
출처: https://inpa.tistory.com/entry/JS-📚-정규식-RegExp-누구나-이해하기-쉽게-정리 [Inpa Dev 👨‍💻:티스토리]
```

#### 캡처하지 않는 그룹화 (?:)

위에서 살펴봤듯이 뜻하지 않은 정규식 그룹화 캡쳐 기능 때문에 쓸데없는 결과값을 얻을 수 있습니다. 이것을 방지하기 위해서는 괄호안에 `?:` 문자를 사용하여 캡쳐기능을 비활성화 할 수 있습니다.

```js
'kokokoko'.match(/(?:ko)+/); // "kokokoko"
```


## 레퍼런스

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_expressions
- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%A0%95%EA%B7%9C%EC%8B%9D-RegExp-%EB%88%84%EA%B5%AC%EB%82%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC
- https://regexr.com/