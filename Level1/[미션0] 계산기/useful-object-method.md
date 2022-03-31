# 유용한 객체 메서드

> 배열 메서드보단 덜 쓰지만 그래도 썼던 것들 모음! 당연히 이것들보단 훨씬 많음

## 정적 메서드

### 1. Object.entries(), Object.keys(), Object.values()

```js
Object.entries(obj);
Object.keys(obj);
Object.values(obj);
```

`obj` : 객체

`return` :

- `entries()` : `obj`의 `[key, value]` 쌍 배열
- `keys()` : `obj`의 `key` 배열
- `values()` : `obj`의 `value` 배열

```js
/* 객체를 순회하고 싶을 때 사용 */
Object.keys(item).forEach((key) => {
  if (key === "name") {
    item[key] = "KIM";
  }
});

Object.entries(item).forEach(([key, value]) => {
  console.log(key, value);
});
```

### 2. Object.freeze()

```js
Object.freeze(obj);
```

`obj` : 동결할 객체

`return` : `freeze()`에 전달된 `obj`, 사본이 아니라 원본 그대로 반환

```js
/* 상수로 분리한 객체를 더 '완벽하게' 지켜야 할 필요가 있을 때 */
const NUMBER_RANGE = Object.freeze({
  MIN: 0,
  MAX: 9,
}); // 이제 속성도 못 바꿈
// -> 오버스펙일 수 있음. 꼭 필요한 게 아니라면 굳이..?
```

---

## 참고 자료

[**MDN: 객체 정적 메서드**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object#%EC%A0%95%EC%A0%81_%EB%A9%94%EC%84%9C%EB%93%9C)

---

## 더 공부하기

- 유용한 배열 메서드
- mutable, immutable

> 메서드 사용 원리가 궁금하다면!

- static
- instance
- prototype, \_\_proto\_\_
