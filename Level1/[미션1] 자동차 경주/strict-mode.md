# strict mode

> JS는 모드같은 것도 있음.. 일부만 지원하거나, 지원하지 않는 브라우저도 있기 때문에 사용 주의!!

> eslint가 거의 다 잡아줌. eslint 사용하자!

## 정의

오류를 발생시킬 가능성이 높거나, 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킴

```js
"use strict";

function foo() {
  x = 10;
}

foo();
```

- `'use strict'` 또는 `"use strict"`를 전역 또는 함수 바디의 앞쪽에 추가

---

## 문제점

- 전역에 적용한 strict mode는 script 단위로 적용되는데, strict mode와 non-strict mode script를 혼용하면 [오류 발생](https://bugzilla.mozilla.org/show_bug.cgi?id=579119) 가능
- 함수에 적용한 strict mode도 마찬가지
- strict mode는 semantic을 바꾸기 때문에, 사용하는 데에 주의해야 함

## strict mode에서 발생하는 에러 케이스

### Reference Error

- 암묵적 전역

```js
(function () {
  "use strict";

  x = 1;
  console.log(x); // ReferenceError: x is not defined
})();
```

### Type Error

- non-strict mode에서 조용히 실패하고 넘어가는 할당

  - 쓸 수 없는 전역 또는 프로퍼티에 할당
  - getter-only 프로퍼티에 할당
  - 확장 불가 객체에 새 프로퍼티 할당

```js
"use strict";

// 1. TypeError: Cannot assign to read only property 'undefined' of object '#<Window>'
var undefined = 5;
var Infinity = 5;
var obj1 = {};
Object.defineProperty(obj1, "x", { value: 42, writable: false });
obj1.x = 9;

// 2. TypeError: Cannot set property x of #<Object> which has only a getter
var obj2 = {
  get x() {
    return 17;
  },
};
obj2.x = 5;

// 3. TypeError: Cannot add property newProp, object is not extensible
var fixed = {};
Object.preventExtensions(fixed);
fixed.newProp = "ohai";
```

- 원시값에 속성 설정

```js
(function () {
  "use strict";

  false.true = ""; // TypeError: Cannot create property 'true' on boolean 'false'
})();
```

### Syntax Error

- 변수, 함수, 매개변수의 삭제

```js
(function () {
  "use strict";

  var x = 1;
  delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.
})();
```

- 매개변수 이름의 중복

```js
(function () {
  "use strict";

  // SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

- 8진 구문

```js
(function () {
  "use strict";
  var sum =
    015 + // SyntaxError: Octal literals are not allowed in strict mode.
    197 +
    142;
})();
```

- with 문의 사용
  > `with`문은 사용하지 않는 게 좋다고 한다.

```js
(function () {
  "use strict";

  // SyntaxError: Strict mode code may not include a with statement
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

## strict mode에서의 변화

1. 일반 함수 `this`

```js
(function () {
  "use strict";

  function foo() {
    console.log(this); // undefined
  }

  foo();
})();
```

- strict mode에서 함수를 일반 함수로 호출하면, `this`에 `undefined`가 바인딩 됨
- 원래는 일반 함수로 호출하면 전역 객체(`window`, `global` 등)가 바인딩 됨

2. `arguments` 객체

```js
(function (x) {
  "use strict";

  console.log(arguments); // Arguments [3, ...]

  x = 0;

  console.log(arguments); // Arguments [3, ...]
})(3);
```

- strict mode에서는 매개변수에 전달된 인자를 재할당해서 변경해도, `arguments` 객체에 반영되지 않음

---

## 참고 자료

[**MDN: strict mode**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode)

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- this

> 자동으로 strict mode인 경우

- class
- module
