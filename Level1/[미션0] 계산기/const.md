# const

> 우테코 미션 할 때 되도록이면 const만 쓰자. let도 줄이자!

## 구문 및 정의

```js
const varname1 = value1 [, varname2 = value2 [, ... [, varnameN = valueN]]];
```

`varname` : 변수명, 식별자

`value` : 변수 초기값

---

## 변수 선언

- 데이터가 담길 메모리 공간 확보
- 확보한 메모리 공간의 주소를 담음
- 변수명(식별자)을 통해 해당 데이터에 접근할 수 있음

## 변수 선언을 해야 하는 이유

### 1. 선언하지 않은 변수는 항상 전역 변수

```js
function x() {
  y = 1;
  var z = 2;
}

x();

console.log(y); // 1
console.log(z); // ReferenceError: z is not defined
```

### 2. 선언하지 않은 변수는 할당문이 실행되기 전까지 존재하지 않음

```js
console.log(a); // ReferenceError: a is not defined
console.log("still going..."); // 실행되지 않음

var a;
console.log(a); // undefined
console.log("still going..."); // 실행됨
```

### 3. 선언하지 않은 변수는 실행 컨텍스트 속성에서 변경 가능(configurable)

```js
var a = 1;
b = 2;

delete this.a; // 실패 -> 선언된 변수는 non-configurable
delete this.b; // 성공 -> b는 실행 컨텍스트(객체) 속성에서 제거됨

console.log(a, b); // ReferenceError: b is not defined
```

## 특징

기본적으로 let의 성질은 모두 갖고 있음

### 1. 선언과 초기화

- 선언과 동시에 초기값을 할당해야 함

```js
const x; // SyntaxError: Missing initializer in const declaration
```

### 2. 재할당 금지

```js
const x = 1;

x = 100; // Uncaught TypeError: Assignment to constant variable
```

### 3. 상수 선언

- JS 상수 특징:
  - 대입 연산자로 재할당 불가능
  - 재선언 불가능
  - 초기값 필요
  - 같은 스코프 내에서 동일한 식별자 사용 불가
  - 상수로 할당된 객체의 속성은 보호되지 않음

```js
const PI = 3.14;

PI = 3.141592; // TypeError: 원시 값은 변경 불가능

const NUMBER = {
  MAX: 9,
  MIN: 0,
};

NUMBER.MIN = 1; // 객체 내부는 변경 가능

NUMBER = 10; // TypeError: 새로운 객체 할당은 불가능
```

---

## 참고 자료

[**MDN: const**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/const)

**『코어 자바스크립트』** **_정재남 지음_**

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- var
- let
- data type
- mutable, immutable
