# promise

> 드디어 비동기를 쓸 때가 왔다..! 주로 promise나 async/await를 쓰게 될 테니 잘 알아두기

## 정의

Promise 객체는 비동기 작업의 완료 또는 실패와 그 결과 값을 나타냄

대기(pending), 이행(flfilled), 거부(rejected) 중 하나의 상태를 가짐

- pending: 이행도 거부도 하지 않은 초기 상태
- fulfilled: 성공적으로 연산 완료
- rejected: 연산 실패
- settled: fulfilled되거나, rejected된 Promise
  - 한 번 settled가 된 Promise 객체는 상태가 변하지 않음

---

## 비동기란?

- 동기(synchronous): 태스크가 순차적으로 실행되는 방식. 현재 실행 중인 태스크가 종료할 때까지 다음 태스크는 대기
  - 실행 순서 보장
  - 태스크 blocking 발생
- 비동기(asynchronous): 현재 실행 중인 태스크가 종료할 때까지 기다리지 않고 다음 태스크를 실행하는 방식. 주로 실행 종료 시간을 예측할 수 없는 네트워크 요청, 이벤트 핸들러, 타이머 함수 등이 비동기 처리 방식으로 동작
  - 실행 순서를 보장하지 않음
  - 태스크 blocking이 발생하지 않음

## JS 비동기 역사 (callback ~ Promise)

> 간단하게만!

### callback 패턴

- 비동기 처리 결과의 후속 처리를 담당
- 비동기 처리가 연쇄적으로 발생하면, callback hell 발생
- callback hell: 콜백 함수 호출이 중첩되어, 복잡도가 높아지는 현상
- 에러 처리가 어려움

  ```js
  try {
    setTimeout(() => {
      throw new Error("Error!!!!");
    }, 0);
  } catch (error) {
    console.error(error.message); // 에러 캐치를 못 함
  }
  ```

  - 콜백이 호출되는 시점에 `setTimeout` 함수는 이미 콜 스택에서 제거된 상태이기 때문에, `catch` 블록에서 에러가 잡히지 않음

- 콜백 제어권을 함수에게 넘기기 때문에, 제어하기 어려움
  - 어떤 시점에 호출할 지, 몇 번 호출할 지 등

### Promise

- ES6에 도입
- callback hell 대신 **chaining**을 이용하여 순차적으로 작성 가능
  - 순차적으로 각각의 작업이 이전 단계 비동기 작업이 성공하고 나서 그 결과값을 이용하여 다음 비동기 작업을 실행해야 하는 경우 유용
- 비동기 흐름 제어권을 넘기지 않음
- 콜백 패턴을 여전히 사용 -> 가독성이 떨어짐
- 여전히 비동기 코드, 동기 코드보단 가독성이 떨어짐
- `resolve`, `reject`도 직접 설정해야 함

## Promise 생성

```js
const executor = (resolve, reject) => {
  if (isSuccess) {
    resolve("success");
    return;
  }
  reject("fail");
};

const promise = new Promise(executor);
```

- Promise 생성자 함수: `new` 연산자로 호출해서 Promise 객체를 생성
- `executor`: 비동기 처리를 수행할 콜백
  - `resolve`: 호출되면 Promise를 fulfilled 상태로 변경
  - `reject`: 호출되면 Promise를 rejected 상태로 변경
- Promise 객체: 비동기 처리 상태와 처리 결과를 갖고 있음
  - `[[PromiseStatus]]`, `[[PromiseValue]]`

## Promise 체이닝

- Promise의 비동기 처리 상태가 변하면 후속 처리 메서드에 전달한 콜백을 선택적으로 호출
- 후속 처리 메서드는 콜백 함수가 반환한 프로미스(또는 반환값을 프로미스로 생성)를 반환

### `Promise.prototype.then`

```js
new Promise((resolve) => resolve("fulfilled")).then(
  (value) => console.log(value),
  (error) => console.log(error)
); // fulfilled

new Promise((_, reject) => reject(new Error("rejected"))).then(
  (value) => console.log(value),
  (error) => console.log(error.message)
); // rejected
```

- fulfilled 상태일 때, 첫번째 콜백 함수 호출
- rejected 상태일 때, 두번째 콜백 함수 호출
- 단, 에러 처리는 `then`보다 `catch`에서 다루는 게 좋음

### `Promise.prototype.catch`

```js
new Promise((_, reject) => reject(new Error("rejected"))).catch((error) =>
  console.log(error.message)
); // rejected
```

- `then`과 동일하게 동작하지만, rejected 상태일 때만 콜백 함수 호출
- 체이닝의 마지막 단계에 호출하면, 비동기 처리 에러뿐만 아니라 `then` 내부에서 발생한 에러까지 모두 캐치할 수 있음

### `Promise.prototype.finally`

```js
new Promise(() => {}).finally(() => console.log("finally")); // finally
```

- settled 상태와 상관 없이 무조건 한 번 콜백 함수 호출

## microtask queue

- 마이크로태스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 임시 저장됨
- 마이크로 태스크 큐의 우선순위는 태스크큐보다 높음

## 기타 메서드

### `Promise.resolve` / `Promise.reject`

```js
Promise.resolve(value);
Promise.reject(reason);
```

- 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용
- `Promise.resove`는 인수로 전달받은 값을 `resolve`하는 Promise 객체를 생성
- `Promise.reject`는 인수로 전달받은 값을 `reject`하는 Promise 객체를 생성

### `Promise.all`

```js
Promise.all(iterable);
```

- 인자로 받은 Promise 객체 배열(또는 iterable)을 모두 병렬 처리
- 비동기 처리가 순차적으로 실행될 필요가 없을 때 사용
- 전달 받은 모든 Promise 객체가 fulfilled 상태가 되면, 모든 처리 결과를 배열에 저장하여 새로운 Promise 객체를 반환
- 인자로 넘겨준 배열 요소 순서와 동일하게 결과값 반환
- 하나라도 rejected 되면 즉시 종료

### `Promise.race`

```js
Promise.race(iterable);
```

- 인자로 받은 Promise 객체 배열(또는 iterable) 요소 중에 가장 먼저 fulfilled 상태가 된 Promise 객체 처리 결과를 resolve하는 새로운 Promise 객체 반환
- 하나라도 rejected 되면 에러를 reject하는 새로운 Promise 객체 반환

### `Promise.allSettled`

```js
Promise.race(iterable);
```

- 전달받은 프로미스가 모두 settled(fullfilled 또는 rejected) 상태가 되면 처리 결과를 배열로 반환
- 반환한 배열의 요소 객체
  - 프로미스가 fulfilled 상태: status, value 프로퍼티 포함
  - 프로미스가 rejected 상태: status, reason 프로퍼티 포함

## 참고 자료

[**MDN: promise**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

[**MDN: using promise**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Using_promises)

[**MDN: using microtask**](https://developer.mozilla.org/ko/docs/Web/API/HTML_DOM_API/Microtask_guide)

**『모던 자바스크립트 Deep Dive』** **_이웅모 지음_**

---

## 더 공부하기

- synchronous vs asynchronous
- async/await
- generator
- iterator
