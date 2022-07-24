# value, expression, statement

> 값, 식, 문이 어떤 차이가 있는 지 알아야 한다! 특히 문과 식을 헷갈리지 않도록 하자.

## 값 - value

- 표현식에 의해 생성된 결과

- 모든 값은 데이터 타입을 가지고, 메모리에 저장됨

  > 메모리에 0100 0001이 저장되어 있더라도, 데이터 타입에 따라 65가 될 수도, 'A'가 될 수도 있음!

- 변수에 할당됨

## 리터럴 - lieteral

- 문자 또는 기호를 사용해 값을 생성하는 표기법

```js
// 숫자 리터럴
10 3.5 0b1010 0o101 0x14

// 문자 리터럴
'hi' "HI" `hihi`

// 불리언 리터럴
true false

// 객체 리터럴
{} {1: 2, 2: 4}

// 배열 리터럴
[] [1, 2]

// 함수 리터럴
function() {}
() => {}

// 정규 표현식 리터럴
/[]/g

// 그 외
null undefined class
```

## (표현)식 - expression

- 값으로 평가될 수 있는 문

- 표현식이 평가되면 새로운 값을 생성하거나, 기존 값을 참조

- 리터럴, 식별자, 연산자, 함수 호출 등의 조합으로 이뤄짐

- 표현식은 값처럼 사용할 수 있음. 즉, 값이 위치할 수 있는 곳엔 표현식도 위치할 수 있음

```js
// 리터럴 표현식
1 '안녕'

// 식별자 표현식
sum
person.name
array[0]

// 연산자 표현식
10 + 20
x = 7 // -> 7로 평가됨

// 함수 호출 표현식
say()
person.getAge()
```

## (명령)문 - statement

- 프로그램을 구성하는 기본 단위이자 최소 실행 단위

- 문은 토큰으로 구성

  - 토큰(token): 문법적으로 더 이상 나눌 수 없는 코드 요소
    - 키워드, 식별자, 리터럴, 연산자 등

- 선언문, 할당문, 조건문, 반복문 등이 있음

## 표현식과 문

- 문에는 값으로 평가되지 않는, 즉 표현식이 아닌 문이 존재

- 표현식이 아닌 문은 변수에 할당할 수 없음

```js
// 선언문
let x; // undefined

let y = let x; // Uncaught SyntaxError: Unexpected identifier

// 할당문
x = 10; // 10

let y = (x = 10); // y는 10을 참조

// 조건문
if(x) {} // undefined

let y = if(x) {}; // Uncaught SyntaxError: Unexpected token 'if'

// 반복문
for(x; x < 10; x++) {} // undefined

let y = for(x; x < 10; x++) {}; // Uncaught SyntaxError: Unexpected token 'for'

```

> 잘 모르겠을 때는 크롬 개발자도구 콘솔창에 입력하면 된다! 문이면 undefined가 뜬다.

---

## 참고 자료

[**MDN: 표현식과 연산자**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators)

[**MDN: 식 및 연산자**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- data type
- first class object
