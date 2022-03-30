# 유용한 배열 메서드

> 우테코 미션 할 때 for문 대신 아래 메서드들을 많이 활용하자! 반복문을 많이 사용하면, 오류 발생 가능성이 증가한다.

## 정적 메서드

### 1. Array.from()

```js
Array.from(arrayLike[, mapFn[, thisArg]])
```

`arrayLike` : 배열로 변환하고자 하는 유사 배열 객체나, 반복 가능한 객체

`mapFn` : 배열의 모든 요소에 대해 호출할 맵핑 함수

`thisArg` : `mapFn` 실행 시에 `this`로 사용할 값

`return` : 새로운 `Array` 인스턴스

```js
/* 단순히 배열로 바꾸고자 할 때 사용 */
const inputList = document.querySelector("input"); // NodeList
const inputArray = Array.from(inputArray);

/* 단순 반복문 대신 사용 */
const numberArray = Array.from({ length: 5 }, (value, index) => index); // [0, 1, 2, 3, 4], value는 항상 undefined
// -> 이 방법 많이 씀!!
```

## 인스턴스 메서드

> 나름 자주 쓰던 순서대로 나열해보았다. 이 외에도 있음!

### 1. map(), forEach()

```js
array.map(callback(currentValue[, index[, array]])[, thisArg])
array.forEach(callback(currentValue[, index[, array]])[, thisArg])
```

`callback` : 각 요소를 시험할 함수

`currentValue` : 처리할 현재 요소

`index` : 처리할 현재 요소의 인덱스

`array` : 메서드를 호출한 배열

`thisArg` : `callback`을 실행할 때 `this`로 사용하는 값

`return` :

- `map()` : `callback`의 결과를 모은 새로운 배열
- `forEach()` : `undefined`

```js
/* 반복문 대신 많이 사용 */
const numberList = [1, 3, 5, 7, 9];
const newNumberList = numberList.map((number) => ++number);
numberList.forEach((number) => {
  console.log(number);
});
// 중간에 멈춰야 한다면, forEach 대신 some, every를 사용하자.
```

### 2. filter

```js
array.filter(callback(currentValue[, index[, array]])[, thisArg])
```

`callback` : 각 요소를 시험할 함수. `true`를 반환하면 요소를 유지하고, `false`를 반환하면 버림

`currentValue` : 처리할 현재 요소

`index` : 처리할 현재 요소의 인덱스

`array` : 메서드를 호출한 배열

`thisArg` : `callback`을 실행할 때 `this`로 사용하는 값

`return` : 테스트를 통과한 요소로 이루어진 새로운 배열

```js
/* 단순 필터링 또는 검색할 때 */
const itemExceptColaList = itemList.filter((item) => item.Name !== "Cola");
// length 속성을 이용해서 개수 셀 때도 사용 가능
```

### 3. every(), some()

```js
array.every(callback(currentValue[, index[, array]])[, thisArg])
array.some(callback(currentValue[, index[, array]])[, thisArg])
```

`callback` : 각 요소를 시험할 함수

`currentValue` : 처리할 현재 요소

`index` : 처리할 현재 요소의 인덱스

`array` : 메서드를 호출한 배열

`thisArg` : `callback`을 실행할 때 `this`로 사용하는 값

`return` :

- `every()` : `callback`이 **모든** 배열 요소에 대해 참인(truthy) 값을 반환하는 경우 `true`, 그 외엔 `false`
- `some()` : `callback`이 **어떤** 배열 요소에 대해 참인(truthy) 값을 반환하는 경우 `true`, 그 외엔 `false`

```js
/* 유효성 판별할 때 사용 */
const isAlreadyExist = itemList.some((item) => input.value === item.Name);

/* 반복문 대신 사용 */
itemList.every((item) => {
  if (item.Name === "Kim") return false; // break 같은 역할!
  if (item.Name === "Son") return true; // continue 같은 역할!
  return true;
});
```

### 4. includes()

```js
array.includes(valueToFind[, fromIndex])
```

`valueToFind` : 탐색할 요소

`fromIndex` : 배열에서 검색을 시작할 위치

`return` : 존재하면 `true`, 그 외엔 `false`

```js
/* 유효성 판별할 때 사용 */
const isAlreadyExist = itemList.includes(name);
```

### 5. join(), split()

> `split()`은 `String` 인스턴스 메서드지만 자주 같이 쓰여서 넣음!

```js
array.join([separator]);
string.split([separator[, limit]])
```

`separator` :

- `join()`: 배열의 각 요소를 구분할 문자열 지정
- `split()`: 원본 문자열을 끊어야 할 부분을 나타내는 문자열

`limit` : 끊어진 문자열의 최대 개수

`return` :

- `join()` : 모든 요소들을 연결한 하나의 문자열을 반환
- `split()` : 주어진 문자열을 분리한 부분 문자열을 담은 배열

```js
const winnerList = ["A", "B", "C", "D"];
const winnerResult = winnerList.join(", "); // 'A, B, C, D'
const winners = winnerResult.split(", "); // ['A', 'B', 'C', 'D']
```

### 6. reduce()

```js
array.reduce(callback(accumulator, currentValue[, currentIndex[, array ]])[, initialValue])
```

`callback` : 각 요소에 대해 실행할 함수

`accumulator` : 콜백의 반환값을 누적

`currentValue` : 처리할 현재 요소

`currentIndex` : 처리할 현재 요소의 인덱스

`array` : 메서드를 호출한 배열

`initialValue` : 최초 호출에서 첫 번째 인수에 제공하는 값

`return` : 누적 계산의 결과 값

```js
/* 합 구할 때 가장 많이 사용 */
const numberList = [1, 2, 3, 4, 5];
const sum = numberList.reduce((acc, number) => acc + number, 0); // 15

/* 오브젝트 초기화 할 때도 사용 */
const numberList = [1, 2, 3, 4, 5];
const initialObj = numberList.reduce((acc, number, index) => {
  acc[index] = number;
  return acc;
}, {});
```

### 7. pop(), push()

```js
array.pop()
array.push(element1[, ...[, elementN]])
```

`element` : 배열의 끝에 추가할 요소

`return` :

- `pop()` : 배열에서 제거한 요소
- `push()` : 호출한 배열의 새로운 length 속성

### 8. slice()

> 얕은 복사를 할 수 있음

```js
array.slice([begin[, end]])
```

`begin` : 추출 시작점에 대한 인덱스

`end` : 추출을 종료 할 인덱스, `end`인덱스를 제외하고 추출

`return` : 추출한 요소를 포함한 새로운 배열

```js
const fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
const citrus = fruits.slice(1, 3); // ['Orange', 'Lemon']
```

### 9. find(), findIndex()

```js
array.find(callback(currentValue[, index[, array]])[, thisArg])
array.findIndex(callback(currentValue[, index[, array]])[, thisArg])
```

`callback` : 각 요소를 시험할 함수

`currentValue` : 처리할 현재 요소

`index` : 처리할 현재 요소의 인덱스

`array` : 메서드를 호출한 배열

`thisArg` : `callback`을 실행할 때 `this`로 사용하는 값

`return` :

- `find()` : 주어진 `callback`을 만족하는 **첫 번째 요소의 값**, 그 외엔 `undefined`
- `findIndex()` : 주어진 `callback`을 만족하는 **첫 번째 요소의 인덱스**, 그 외엔 `-1`

```js
/* 요소 찾을 때 사용 */
const selectedItem = itemList.find((item) => item.Name === input.value);
const selectedItemIndex = itemList.findIndex((item) => item.Name === input.value);

/* 탐색 중 삭제된 배열 요소에도 callbcak 호출됨 */
itemList.find((value, index) => {
  if (index == 0) {
    delete a[5];
  }
  console.log(`index: ${index}, value: ${value}`); // 5번째 인덱스에서 value: undefined가 출력됨
}
```

---

## 참고 자료

[**MDN: 배열 정적 메서드**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array#%EC%A0%95%EC%A0%81_%EB%A9%94%EC%84%9C%EB%93%9C)

[**MDN: 배열 인스턴스 메서드**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array#%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4_%EB%A9%94%EC%84%9C%EB%93%9C)

---

## 더 공부하기

- 유용한 객체 메서드
- mutable, immutable

> 메서드 사용 원리가 궁금하다면!

- static
- instance
- prototype, \_\_proto\_\_
