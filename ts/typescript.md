# 타입스크립트를 사용하는 이유

## 타입스크립트란 ?
- 자바스크립트의 확장된 언어라고 볼 수 있으며, 자바스크립트에 타입을 부여한 언어 입니다. <br>
브라우저에서는 타입스크립트를 실행 할 수 없기 때문에 __컴파일(complile)__ 과정을 거쳐야 합니다.

<br>

## TypeScript vs JavaScript
|  ||
|---|:---:|
|TypeScript|JavaScript|
동적타입 언어 |	정적타입 언어 
인터프리터 언어 |	컴파일 언어 
자바스크립트에 의존적임  | 독립적으로 사용 가능 
더 나은 구조와 간결함, 일관성, 재사용성 | 좀 더 유연함 (타입에 제한을 받지 않으므로)
.ts 확장자 | .js 확장자	
복잡한 프로젝트에 적합함 | 작고 간단한 프로젝트에 적합함	

<br>

## 타입스크립트 쓰는 이유

1. 에러의 사전 방지
```js
// math.js
function sum(a, b) {
  return a + b;
}

// math.ts
function sum(a: number, b: number) {
  return a + b;
}
```

- 자바스크립트 같은 경우는 
```js
sum(10, 20); // 30
sum('10', '20'); // 1020
```
- 위와 같이 실행이 됩니다.
- 타입이 동적으로 바뀌기 때문에 사용자가 의도치 않게 사용을 하여도 에러가 나지 않습니다.

<br>

- 타입스크립트 같은 경우는
```js
// math.ts
function sum(a: number, b: number) {
  return a + b;
}
sum('10', '20'); // Error: '10'은 number에 할당될 수 없습니다.
```
- 타입을 명시하기 때문에 의도치 않은 사용을 사전에 예방 할 수 있습니다.

<br>

2. 개발 생산성 향상
```js
// math.js
function sum(a, b) {
  return a + b;
}
var total = sum(10, 20);
total.toLocaleString();
```
- 자바스크립트에서 `total` 에 `toLocalString` API 를 사용하기 위해서는 자동완성이 되지 않기 때문에 <br> 사용자가 `number` 라는 타입으로 가정하에 직접 입력해야 합니다.

- 하지만 타입스크립트는, 애초에 `number` 라는 타입을 선언하기 때문에 각 타입에 알맞은 API 가 <br> 자동완성을 통해서 사용자가 쉽게 접근을 할 수 있습니다.

<br>

## 참고
- [캡틴판교](https://joshua1988.github.io/ts/why-ts.html#%EC%99%9C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%8D%A8%EC%95%BC%ED%95%A0%EA%B9%8C%EC%9A%94)
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8)
- https://imagineu.tistory.com/6