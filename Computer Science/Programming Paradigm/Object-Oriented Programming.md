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

오버라이딩(overriding)은 주로 메서드 오버라이딩(method overriding)을 말하며 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것을 의미합니다.

이는 런타임 중에 발생하는 '동적' 다형성입니다.

```java
class Animal {
  public void bark() {
    System.out.println("mumu! mumu!");
  }
}

class Dog extends Animal {
  @Override
  public void bark() {
    System.out.println("wal!!! wal!!!");
  }
}

public class Main {
  public static void main(String[] args) {
    Dog d = new Dog();
    d.bark();
  }
}

/*
wal!!! wal!!!
*/
```

앞의 코드를 보면 부모 클래스는 mumu! mumu!로 짖게 만들었지만 자식 클래스에서 wal!!! wal!!!로 짖게 만들었더니 자식 클래스 기반으로 메서드가 재정의됨을 알 수 있습니다.

## 설계 원칙

객체지향 프로그래밍을 설계할 때는 SOLID 원칙을 지켜주어야 합니다. S는 단일 책임 원칙, O는 객방-폐쇄 원칙, L은 리스코프 치환 원칙, I는 인터페이스 분리 원칙, D는 의존 역전 원칙을 의미합니다.

### 단일 책임 원칙

단일 책임 원칙(SRP, Single Responsibility Principle)은 모든 클래스는 각각 하나의 책임만 가져야 하는 원칙입니다. 예를 들어 A라는 로직이 존재한다면 어떤한 클래스는 A에 관한 클래스여야 하고 이를 수정한다고 했을 때도 A와 관련된 수정이어야 합니다.

### 개방-폐쇄 원칙

개방-폐쇄 원칙(OCP, Open Closed Principle)은 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 하는 원칙입니다. 즉, 기존의 코드는 잘 변경하지 않으면서도 확장은 쉽게 할 수 있어야 합니다.

### 리스코프 치환 원칙

리스코프 치환 원칙(LSP, Liskov Substritution Principle)은 프로그램의 객체는 프로그래밍의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것을 의미합니다. 클래스는 상속이 되기 마련이고 부모, 자식이라는 계층 관계가 만들어집니다. 이때 부모 객체에 자식 객체를 넣어도 시스템이 문제없이 돌아가게 만드는 것을 말합니다.

### 인터페이스 분리 원칙

인터페이스 분리 원칙(ISP, Interface Segregation Principle)은 하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 하는 원칙을 말합니다.

### 의존 역전 원칙

의존 역전 원칙(DIP, Dependency Inversion Principle)은 자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않게 하는 원칙을 말합니다. 예를 들어 타이어를 갈아끼울 수 있는 틀을 만들어 놓은 후 다양한 타이어를 교체할 수 있어야 합니다. 즉, 상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립해야 합니다.