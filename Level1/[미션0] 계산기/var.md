# var

> 웬만하면 쓰지 말자 ㅎㅎ

## 구문 및 정의

```js
var varname1 [= value1 [, varname2 [, varname3 ... [, varnameN]]]];
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

#### 1. 중복 선언 허용

- 같은 스코프 내에서도 중복 선언 허용

```js
var x = 1;
var y = 1;

var x = 100;
var y; // 초기화문이 없는 변수 선언문은 무시

console.log(x); // 100
console.log(y); // 1
```

#### 2. 함수 레벨 스코프

- 오직 함수의 블록만을 로컬 스코프로 인정
- 함수 내부가 아닌 if문, for문 등의 블록 내에서 선언하면 전역 변수

```js
var x = 1; // 전역 변수

// if문 블럭
if (true) {
  var x = 100; // 전역 변수
}

console.log(x); // 100
```

#### 3. 변수 호이스팅

- 마치 변수 선언문이 코드의 맨 윗단으로 끌어 올려진 것처럼 동작하는 현상
- 코드가 실행되기 전, 자바스크립트 엔진은 이미 실행 환경의 변수명을 알고 있음(평가 과정)
- var는 평가 과정에서, 선언 단계와 초기화 단계(undefined로 초기화)가 동시에 이뤄짐

```js
console.log(x); // undefined

x = 100;

console.log(x); // 100

var x; // 호이스팅 발생

console.log(x); // 100
```

### 문제점

#### 1. 중복 선언 허용으로 인한 예상치 못한 결과 발생

```js
/* 개발자 A가 작성한 코드 */
var answer = "이 문장은 바뀌면 안 됩니다.";

/* ... 수만 줄의 코드 ... */

/* 개발자 B가 작성한 코드 */
var answer = 1;

console.log(answer); // 1
```

#### 2. 함수 레벨 스코프로 인한, 의도치 않은 전역 변수 선언

#### 3. 호이스팅으로 인해 떨어지는 가독성

```js
/* 실행 순서대로 재배치 */
var x;

console.log(x); // undefined

x = 100;

console.log(x); // 100

console.log(x); // 100
```

#### 4. 선언 전 호출이 가능하기 때문에, 오류 발생 가능성 증가

---

## 예제

```js
/* 여러 변수 선언 및 초기화 */
var x, y;
var a = 0,
  b = 0;

/* 단일 값으로 여러 변수 할당 */
var a = "A";
var b = a;
// 또는
var a,
  b = (a = "A");
// 순서 주의
var x = y,
  y = "A"; // x: undefined, y: "A"
```

---

## 참고 자료

[**MDN: var**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/var)

**『코어 자바스크립트』** **_정재남 지음_**

---

## 더 공부하기

- let
- const
- scope
- execution context
