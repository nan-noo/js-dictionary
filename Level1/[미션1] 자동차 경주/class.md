# class

> 구조 설계할 때나, 구현할 때 클래스를 본격적으로 사용할 텐데, 뭔지는 알고 쓰자!

## 정의

인스턴스를 생성하기 위한 틀, 템플릿

JS에서 클래스는 프로토타입을 이용해서 만들어짐

JS 클래스는 함수 객체

```js
// 1. 클래스 선언식
class Rectangle {
  ...
}

// 2. 클래스 표현식
let Rectangle = class {
  ...
};
console.log(typeof Rectangle); // function
```

---

## 특징

1. 호출시 `new` 연산자 필수
2. 상속 지원 키워드(`extends`, `super`) 제공
3. TDZ(Temporal Dead Zone) 존재
4. 재정의 불가능
5. 클래스 바디의 모든 코드는 strict mode
6. 클래스의 모든 속성은 열거 불가능(`[[Enumerable]]`이 `false`)

## 메서드(method)

1. 메서드 축약 표현 사용
2. 콤마(,) 필요 없음
3. strict mode
4. 열거 불가능
5. non-constuctor

### constructor

인스턴스를 생성하고 초기화하기 위한 특수 메서드

```js
class Rectangle {
  // constructor 메서드
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
}
```

- 이름 변경 불가능
- 클래스 정의가 평가된 후, `constructor` 메서드에 작성된 동작을 하는 함수 객체가 생성됨
- 클래스 내에 최대 한 개
- 생략할 시, 빈 `constructor`가 정의됨
- 내부에서 `this`에 인스턴스 속성을 추가할 수 있음
- 암묵적으로 `this`에 바인딩 된 인스턴스를 반환하기 때문에, 별도의 반환문이 없어야 함

### 프로토타입(prototype) 메서드

인스턴스가 상속받아 사용하는 메서드

```js
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // prototype 메서드
  getArea() {
    return this.width * this.height;
  }
}

const rectangle = new Rectangle(2, 3);
console.log(rectangle.getArea()); // 6
```

- 클래스 바디에 작성
- 클래스의 `prototype` 객체의 속성
- 인스턴스 `[[Prototype]]`이 클래스 `prototype` 참조
- 호출시 메서드 내부의 `this`는 인스턴스를 가리킴

### 정적(static) 메서드

인스턴스를 생성하지 않고, 클래스로 호출하는 메서드

```js
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // static 메서드
  static sayType() {
    console.log("직사각형!");
  }

  getArea() {
    return this.width * this.height;
  }
}

Rectangle.sayType(); // 직사각형!
```

- 메서드에 `static` 키워드 사용
- 클래스 자체 속성
- 인스턴스의 프로토타입 체인을 따라 접근할 수 없음
- 메서드 내에서 인스턴스 속성을 참조할 수 없음
  > airbnb eslint rule을 사용하다보면, this를 사용하지 않는 메서드를 static으로 바꾸라는 경고가 뜨는데, 요런 특징 때문이기도 함!
- 호출시 메서드 내부의 `this`는 클래스를 가리킴
- util 함수를 전역으로 정의하지 않고, 메서드로 구조화할 때 유용
  > 그런데, 어떤 클래스가 static method로만 이뤄져 있다면 굳이 클래스를 써야할지 생각해보기! 그냥 객체나 함수로 충분하지 않을까?

## 속성(property)

### 인스턴스(instance) 속성

말 그대로, 인스턴스의 속성

```js
class Rectangle {
  constructor(width, height) {
    // instance 속성
    this.width = width;
    this.height = height;
  }
}
```

- `constructor` 내부에서 `this`에 추가
- 특정 키워드가 없으면 언제나 `public`

### 접근자(getter, setter) 속성

자체적인 값 없이, 다른 데이터 속성의 값을 읽거나 변경할 때 사용하는 접근자 함수(accessor function)로 구성

```js
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // getter
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  }
}

const person = new Person("KIM", "TAETAE");
console.log(person.fullName); // KIM TAETAE

person.fullName = "KIM NANNOO";
console.log(person.fullName); // KIM NANNOO
```

- 프로토타입의 속성
- getter: 인스턴스 속성에 접근할 때, 속성 값을 조작하거나 별도의 행위가 필요할 때 사용. 반환값 필수
- setter: 인스턴스 속성 값을 변경할 때, 속성 값을 조작하거나 별도의 행위가 필요할 때 사용. 매개변수(1개) 필수

## field(member) 선언

> 실험적 기능이지만, 크롬 등에선 쓸 수 있음. Java같은 다른 언어에선 이미 사용 중! TS는 기본적으로 사용하는 듯..?

- 모든 클래스 필드는 인스턴스의 속성
- 필드 참조 시 `this`로 접근 필수
- 클래스 바디에 필드 선언 시에는 `this` 불필요
- 외부 값으로 필드를 초기화할 때는 `constructor` 내부에서 초기화
  > 이런 경우엔 굳이 필드로 선언할 필요 없음(self-documenting 목적이 아니라면)
- 클래스 필드로 메서드 정의 가능

  > 단, 이렇게 하면 프로토타입 메서드가 아닌 인스턴스 메서드가 됨!! 인스턴스가 많아질수록 메모리를 많이 쓰게 됨. -> 메서드를 필드로 작성하는 것은 권장하지 않음.

  > 필드로 화살표 함수를 선언해서 this 바인딩을 해결할 수도 있음. 이벤트리스너 콜백 함수 필드 메서드로 정의할 때 많이 썼음!

- `static` 키워드 사용 가능

### public

특정 키워드가 없으면 항상 public

```js
class Rectangle {
  type = "직사각형";
}

const rectangle = new Rectangle();
console.log(rectangle.type); // 직사각형
```

- 클래스 내부, 자식 클래스 내부, 클래스 인스턴스에서 접근 가능

### private

`#`을 붙여 클래스 외부에서 참조할 수 없도록 함

```js
class Rectangle {
  #type = "직사각형";

  constructor() {
    console.log(this.#type); // 직사각형
  }

  get type() {
    return this.#type;
  }
}

const rectangle = new Rectangle();
console.log(rectangle.#type); // SyntaxError: Private field '#type' must be declared in an enclosing class
console.log(rectangle.type); // 직사각형
```

- 해당 필드가 선언된 클래스(자식 클래스 불포함)에서만 접근 가능
- 접근자 함수를 통해 간접 접근 가능

## 상속

기존 클래스를 확장해 새로운 클래스를 정의

> extends와 implements는 다른 거! js에서 클래스 상속(extends)을 interface implements처럼 사용하지 말장

> 상속은 주로 여러 클래스가 공통된 부분이 있을 때 중복을 줄이기 위해 쓰는 것 같음

```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  eat() {
    console.log(`${this.name} eat`);
  }

  move() {
    console.log(`${this.name} move`);
  }
}

class Bird extends Animal {
  fly() {
    console.log(`${this.name} fly`);
  }
}

const bird = new Bird("독수리");

console.log(bird.eat(), bird.move(), bird.fly()); // 독수리 eat, 독수리 move, 독수리 fly
```

- `extends` 키워드로 상속
- 부모 클래스 속성을 물려받음 + 자신만의 고유 속성 추가
- 코드 재사용 관점에서 유용
- 인스턴스 프로토타입 체인뿐만 아니라, 클래스 프로토타입 체인도 생성: static, prototype 메서드와 속성 모두 상속 가능

### super

부모클래스 `constructor` 호출(`super()`) 또는, 부모클래스 메서드 호출(`super.method()`) 가능

```js
class Parent {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class Child extends Parent {
  constructor(x, y, z) {
    super(x, y);
    this.z = z;
  }
}
```

- 자식클래스 `constructor`를 생략하지 않는 경우, `constructor`에서 호출 필수
- 호출 전에 `this` 참조 불가능

> 자식클래스는 인스턴스를 직접 생성하지 않고, 부모클래스에게 super를 호출해서 인스턴스 생성 과정을 위임하기 때문

---

## 참고 자료

[**MDN: class**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- prototype
- prototype chain
- strict mode
