# 모듈화
다른 파일에 있는 자바스크립트의 기능을 특정 파일에서 사용할 수 있는 것을 의미합니다.

<br>

## Import & Export
- 임포트(Import)와 익스포트(Export)는 자바스크립트의 코드를 모듈화 할 수 있는 기능

<br>

### 기본 문법
```js
export 변수, 함수
```
1. 다른 파일에서 가져다 쓸 변수나 함수의 앞에 `export` 라는 키워드를 붙입니다.

2. 익스포트된 파일은 임포트로 불러와 사용할 수 있습니다.
```js
import { 불러올 변수 또는 함수 이름 } from '파일 경로';
```

<br>

### 예제
- math.js
```js
export let pi = 3.14;

export function sum(a, b) {
  return a + b;
}
```
<br>

- app.js
```js
import { pi } from './math.js';
console.log(pi); // 3.14

import { sum } from './math.js';
sum(10, 20); // 30
```

<br>

## 사용하는 이유
- 자바스크립트의 유효 범위는 전역으로 시작 합니다.
- 파일마다 고유의 영역이 나누어 지지 않기 때문에 같은 변수로 이름을 작성하면 변수의 유효범위가 분리되지 않기 떄문에

- 변수가 덮어 씌어지거나 예상치 못한 상황을 만날 수 있습니다.
<br>

```html
<!-- index.html -->
<body>
  <script src="a.js"></script>
  <script src="b.js"></script>
  <script>
    getTotal(); // 200
  </script>
</body>
```
```js
// a.js
var total = 100;
function getTotal() {
  return total;
}
```

```js
var total = 200;
```

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/vue-camp/es6+/modules.html#%E1%84%86%E1%85%A9%E1%84%83%E1%85%B2%E1%86%AF%E1%84%92%E1%85%AA%E1%84%8B%E1%85%B4-%E1%84%91%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%AD%E1%84%89%E1%85%A5%E1%86%BC)