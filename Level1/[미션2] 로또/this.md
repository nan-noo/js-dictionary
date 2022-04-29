# this

> `this` 잘 모르면 이벤트 핸들러 달면서 문제 꼭 겪게 됨ㅎㅎ 자세히 알아두기!!

## 구문 및 정의

```js
this;
```

실행 컨텍스트 속성으로, non-strict mode에선 항상 객체를 참조하고, 엄격 모드는 어떤 값이든 될 수 있음

**함수를 호출한 주체**의 정보를 담는 속성, 자기 참조 변수

`this`가 가리키는 값은 함수 호출 방식에 따라 결정

---

## 컨텍스트에 따른 this

### 전역

```js
console.log(this); // Window
```

- strict mode 여부에 관계 없이, 항상 전역 객체를 참조
- `globalThis` 속성으로 현재 실행 컨텍스트와 관계 없이 항상 전역 객체 참조 가능
- 브라우저는 `window`, node.js 환경에서는 `global`

### 함수

1. 단순 호출

```js
function foo() {
  console.log(this);
}

foo(); // Window
```

```js
function foo() {
  "use strict";
  console.log(this);
}

foo(); // undefined
```

- non-strict mode: 전역 객체 참조
  > 이 부분 때문에 많이 헷갈림. 설계상의 오류라고도 함!
- strict mode: `undefined`

2. 메서드로 호출

```js
function foo() {
  console.log(this);
  console.log(this.myName);
}

const obj = {
  myName: "TaeTae",
  method: foo,
};

foo(); // Window, undefined
obj.method(); // {myName: 'TaeTae', method: ƒ}, TaeTae
obj["method"](); // {myName: 'TaeTae', method: ƒ}, TaeTae
```

- `.` 또는 `[]` 앞의 객체를 참조

3. 생성자 함수로 호출

```js
function Foo(myName) {
  this.myName = myName;
  console.log(this);
  console.log(this.myName);
}

Foo("TaeTae"); // Window, TaeTae
const foo = new Foo("TaeTae"); // Foo {myName: 'TaeTae'}, TaeTae
console.log(foo.myName); // TaeTae
```

- `new` 키워드로 호출시, 생성된 instance를 `this`에 바인딩
- class도 마찬가지

4. 콜백 함수로 호출

```js
function callback(e) {
  console.log(this); // <div ...>...</div>
  console.log(e.currentTarget); // <div ...>...</div>
  console.log(this === e.currentTarget); // true
}

const div = document.querySelector("div");
div.addEventListener("click", callback);
```

- 제어권을 가진 함수가 콜백에 `this`를 지정하는 경우에는 그 값 - 아닌 경우에는 단순 호출과 같음
- `addEventListener` 메서드는 자신의 `this`를 넘겨주도록 정의되어 있기 때문에, 콜백함수의 `this`도 `.` 앞의 요소가 됨

5. 화살표 함수로 호출

```js
function Foo() {
  this.myName = "TaeTae";
  this.arrow = () => {
    console.log(this);
    console.log(this.myName);
  };
}

const foo = new Foo();
foo.arrow(); // Foo {myName: 'TaeTae', arrow: ƒ}, TaeTae

const obj = {
  myName: "TaeTae",
  arrow: () => {
    console.log(this);
    console.log(this.myName);
  },
};

obj.arrow(); // Window, undefined
```

- 자체 `this`를 가지고 있지 않아서, 자신을 감싼 lexical scope의 `this`를 참조
- lexical scope: 함수가 **정의된 환경(위치)** 에 따라 상위 스코프에 대한 참조를 결정
  > 이 속성 때문에, 클래스에서 이벤트 리스너 콜백 함수를 정의할 때 프로토타입 메서드가 아닌, 화살표 함수(필드)로 정의하기도 함

## 명시적 this 바인딩

### `bind`

```js
func.bind(thisArg[, arg1[, arg2[, ...]]]);
```

`func`: 타겟 함수
`thisArg`: `func`의 `this`에 전달하는 값
`arg`: `func`의 매개변수 앞에 사용될 인자
`return`: 지정한 `this` 값 및 초기 인자를 사용하여 변경한 원본 함수(`func`)의 복제본

### `call`

```js
func.call(thisArg[, arg1[, arg2[, ...]]]);
```

`func`: 타겟 함수
`thisArg`: `func`의 `this`에 전달하는 값
`arg`: `func`의 매개변수 앞에 사용될 인자
`return`: `this`와 `arguments`를 매개로 호출된 함수의 반환값

### `apply`

```js
func.apply(thisArg, [argsArray]);
```

`func`: 타겟 함수
`thisArg`: `func`의 `this`에 전달하는 값
`argsArray`: `func`의 인자를 지정하는 유사 배열 객체
`return`: `this`와 `arguments`를 매개로 호출된 함수의 반환값

### 인자로 `this`를 받는 경우

- 주로 반복 수행하는 배열 메서드(`forEach`, `map`, `filter`, ...)의 생략 가능한 두 번째 매개 변수(`thisArg`)에 지정

---

## 참고 자료

[**MDN: this**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)

**『코어 자바스크립트』** **_정재남 지음_**

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- scope
- execution context
- function
  - constructor function
  - arrow function
  - callback function
  - method
