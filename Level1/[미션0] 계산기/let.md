# let

> 얘보단 const!

## 구문 및 정의

```js
let varname1 [= value1] [, varname2 [= value2]] [, ..., varnameN [= valueN]];
```

`varname` : 변수명, 식별자

`value` : 변수 초기값

---

## 설명

### 변수 선언

- 데이터가 담길 메모리 공간 확보
- 확보한 메모리 공간의 주소를 담음
- 변수명(식별자)을 통해 해당 데이터에 접근할 수 있음

### 특징

#### 1. 중복 선언 금지

- 같은 스코프 내에서 중복 선언 금지

```js
let x = 1;
let x = 100; // Uncaught SyntaxError: Identifier 'x' has already been declared
```

#### 2. 블록 레벨 스코프

- 자신을 선언한 블록을 로컬 스코프로 인정

```js
let x = 1;

// 블록으로 인해 새로운 스코프 생성
if (true) {
  let x = 100;
  console.log(x); // 100
}

console.log(x); // 1
```

#### 3. TDZ(Temporal Dead Zone)

- let으로 선언된 변수 스코프의 최상단에서 변수 초기화 완료 시점까지
- 코드 작성 순서가 아닌, 코드 실행 순서에 의해 형성
- 발생 이유
  - 코드가 실행되기 전, 자바스크립트 엔진은 이미 실행 환경의 변수명을 알고 있음(평가 과정)
  - let은 평가 과정에서, 선언 단계만 이뤄짐(초기값이 할당되지 않음)
  - 초기값 할당은 선언문이 실행될 때 이뤄짐

```js
console.log(x); // Uncaught ReferenceError: x is not defined

let x; // 초기값이 지정되어 있지 않으므로 undefined로 초기화

console.log(x); // undefined

x = 1;

console.log(x); // 1
```

---

## 예제

```js
/* 전역에 let 선언 */
var x = "global";
let y = "global"; // 전역 객체에 속성을 추가하지 않음

console.log(this.x); // "global"
console.log(this.y); // undefined

/* var와 let 같이 사용 */
let x = 1;
var x = 2; // 재선언으로 인한 SyntaxError
// 순서 반대로 선언해도 마찬가지(호이스팅)
```

---

## 참고 자료

[**MDN: let**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)

**『코어 자바스크립트』** **_정재남 지음_**

---

## 더 공부하기

- var
- const
- scope
- execution context
