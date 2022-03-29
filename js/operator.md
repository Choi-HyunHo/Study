# ==(동등 연산자)와 ===(일치 연산자)의 차이

동등연산자(==)는 피연산자들의 값만 비교합니다.
- 즉, == 연산자는 타입 변환이 필요한 경우 타입 변환을 한 후에 동등하지 비교한다.
<br>

일치연산자(===)는 피연산자들의 값과 타입을 모두 비교합니다.
- 즉, === 연산자는 타입 변환을 하지 않고 두 값이 같은 타입이 아닌 경우 ===는 false를 반환한다.

<br>

## 예시
```js
console.log('01' == 1) // true, 문자열 '01'이 숫자 1로 변환된 후 비교 진행
console.log(true == 1) // true, true 는 1, false 는 0으로 변환된 후 비교
console.log(false == 0) // true

console.log(0 === false) // false
```

<br>

## null이나 undefined와 비교
- 변수에 값이 `null` 이라면 변수가 선언되고 `null` 이라는 값이 주어진 상태

- `undefined` 라면 변수가 선언되고 아무것도 하지 않은 상태

- `null` 은 직접적으로 값이 없어라고 말한 상태이지만 `undefined` 는 아무것도 하지 않은 상태
```js
console.log(null == undefined) // true
console.log(null === undefined) // false
```


<br>

## 참고
- [MDN, 동치 비교 및 동일성](https://developer.mozilla.org/ko/docs/Web/JavaScript/Equality_comparisons_and_sameness)
- [동등 연산자와 일치 연산자](https://velog.io/@najiexx/JavaScript-%EC%99%80-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EB%8F%99%EB%93%B1%EC%97%B0%EC%82%B0%EC%9E%90%EC%99%80-%EC%9D%BC%EC%B9%98%EC%97%B0%EC%82%B0%EC%9E%90)
- https://7942yongdae.tistory.com/45
- [드림코딩](https://www.youtube.com/watch?v=OCCpGh4ujb8&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=3)