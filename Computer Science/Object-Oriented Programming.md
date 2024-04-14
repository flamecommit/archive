# 객체지향 프로그래밍 (OOP)

객체지향 프로그래밍은 객체들의 집합으로 프로그래밍의 상호 작용을 표현하며 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식을 말합니다. 설계에 많은 시간이 소요되며 처리 속도가 다른 프로그래밍 패러다임에 비해 상대적으로 느립니다.

예를 들어 자연수로 이루어진 배열에서 최대값을 찾으라고 한다면 다음과 같이 로직을 구성합니다.

```js
const ret = [1, 2, 3, 4, 5, 11, 12]

class List {
  constructor(list) {
    this.list = list;
    this.mx = list.reduce((max, num) => num > max ? num : max, 0)
  }
  getMax() {
    return this.mx;
  }
}

const a = new List(ret);
console.log(a.getMax()) // 12
```

클래스 List의 메서드 getMax()로 list의 최댓값을 반환하는 예제입니다.

## 객체지향 프로그래밍의 특징

객체지향 프로그래밍은 추상화, 캡슐화, 상속성, 다형성이라는 특징이 있습니다.

### 추상화

추상화(abstraction)란 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것을 의미합니다. 예를 들어 필자의 후배 종화에게는 군인, 장교, 키180, 여친있음, 안경씀, 축구못함, 롤마스터티어 등의 특징이 있습니다. 이러한 특징 중에서 코드로 나타낼 때 일부분의 특징인 군인, 장교만 뽑아내거나 조금 더 간추려서 나타내는 것을 말합니다.

### 캡슐화

캡슐화(encapsulation)는 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것을 말합니다.

### 상속성

상속성(inheritance)은 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것을 말합니다. 코드의 재사용 측면, 계층적인 관계 생성, 유지 보수성 측면에서 중요합니다.

### 다형성

다형성(polymorphism)은 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것을 말합니다. 대표적으로 오버로딩, 오버라이딩이 있습니다.

#### 오버로딩

오버로딩(overloading)은 같은 이름을 가진 메서드를 여러 개 두는 것을 말합니다. 메서드의 타입, 매개변수의 유형, 개수 등으로 여러 개를 둘 수 있으며 컴파일 중에 발생하는 '정적' 다형성입니다.

```java
class Person {
  public void eat(String a) {
    System.out.println("I eat " + a); 
  }

  public void eat(String a, String b) {
    System.out.println("I eat " + a + " and" + b);
  }
}

public class CalculateArea {
  public static void main(String[] args) {
    Person a = new Person();
    a.eat("apple");
    a.eat("tomato", "phodo");
  }
}

/*
I eat apple
I eat tomato and phodo
*/
```

앞의 코드를 보면 매개변수의 개수에 따라 다른 함수가 호출되는 것을 알 수 있습니다.

#### 오버라이딩
