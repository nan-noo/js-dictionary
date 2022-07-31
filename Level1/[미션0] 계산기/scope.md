# scope

> JS 기본 개념!

## 구문 및 정의

- 식별자가 유효한 범위
- 만약 어떤 식별자가 현재 스코프(또는 상위 스코프)에 존재하지 않으면 사용할 수 없음
- 계층 구조로 이루어져 있어, 하위 스코프는 상위 스코프에 접근할 수 있지만 상위 스코프는 하위 스코프에 접근할 수 없음

---

## 렉시컬 스코프(lexical scope, static scope)

- 함수가 **정의된 위치**에 따라 함수의 상위 프코프를 정적으로 결정하는 방식
- JS에서는 렉시컬 스코프에 따라 함수의 상위 스코프가 결정됨
- dynamic scope(동적 스코프): 함수가 호출된 위치에 따라 상위 스코프를 동적으로 결정

## 스코프 종류

```js
// 1. global
let a = "global";

function foo() {
  // 3. function
  var b = "hi";
  let a = "function";

  if (true) {
    // 3. function
    var b = "hello";
    // 4. block
    let a = "block";

    console.log(a); // block
  }

  console.log(b); // hello
  console.log(a); // function
}

console.log(a); // global
console.log(b); // ReferenceError: b is not defined
```

### 1. 전역 스코프(global scope)

- 최상위 스코프

### 2. 모듈 스코프(module scope)

- 모듈 모드를 사용할 때 모듈들은 자체 스코프를 가짐

### 3. 함수 스코프(function scope)

- var 키워드가 지원하는 스코프

### 4. 블록 스코프(block scope)

- let, const 키워드가 지원하는 스코프

## 스코프 체인(scope chain)

- 스코프가 계층적으로 연결된 것
- 하위 스코프부터 상위스코프까지 차례로 검색해 나가는 것

### outerEnvironmentReference

- 스코프 체인을 가능하게 함
- 현재 실행 컨텍스트가 선언될 당시의 lexicalEnvironment를 참조

### 변수 은닉화(variable shadowing)

- 서로 다른 스코프에 같은 이름의 식별자가 존재할 때, 하위 스코프에서 상위 스코프의 식별자에 접근할 수 없는 현상
- JS엔진은 스코프 체인을 따라 식별자를 검색해 나가는데, 검색하는 도중 해당하는 식별자를 만나면 검색을 중단함
- 따라서 상위 스코프의 식별자에 접근할 수 없음

---

## 참고 자료

[**MDN: scope**](https://developer.mozilla.org/en-US/docs/Glossary/Scope)

**『코어 자바스크립트』** **_정재남 지음_**

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- execution context
- var
- let
- const
- module
