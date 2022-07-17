# 일급 객체

> JS 함수가 일급 객체라는 소리 들어보긴 했는데.. 하지 말고 알아두자!

## 정의

- 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체

- 다음의 조건을 만족해야 함

  0. 함수가 일급 객체가 되기 위한 조건: 런타임에 생성 가능
     > 확실한 조건은 아닌 것 같다. 좀 더 조사가 필요!
  1. 변수에 할당 가능
  2. 함수 인자로 전달 가능
  3. 함수 반환값으로 사용 가능

## 일급 함수

- 자바스크립트의 함수는 일급 객체의 조건을 모두 만족하므로 일급 객체
  - 즉, 함수를 객체와 동일하게 사용할 수 있음
  - 다른 객체처럼 값으로 취급 가능
  - 다른 객체와 차이점은, 함수는 호출할 수 있음
- JS 함수는 값을 사용할 수 있는 곳이라면 항상 리터럴로 정의할 수 있으며, 런타임에 함수 객체로 평가됨

### 특징

1. 변수에 함수 할당

```js
const foo = function () {};
const bar = () => {};
const funcs = [foo, bar];

foo(); // 변수에 할당하여 함수를 호출할 수 있음
bar();
```

2. 함수를 인자(argument) 및 매개변수(parameter)로 사용

```js
function foo(func) {
  // parameter
  func();
}

function bar() {}

foo(bar); // argument -> 여기서 bar 함수는 콜백함수
```

3. 함수를 함수 반환값으로 사용

```js
// foo는 고차 함수
function foo() {
  return function () {};
}

foo(); // function () {}
```

---

## 참고 자료

[**MDN: 일급 함수**](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)

[**MDN: 함수**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions)

[**Wiki: 일급 객체**](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- value, expression, statement

- function

- argument, parameter

- callback

- higher order function

- closure
