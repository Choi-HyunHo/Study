# 타입스크립트의 모듈
ES6 와 비슷 합니다.
모듈은 전역 변수와 구분되는 자체 유효 범위를 가지며<br> `export, import` 와 같은 키워드를 사용하지 않으면 다른 파일에서 접근할 수 없습니다.

<br>

## 사용방법
### Export
- ES6의 `export`와 같은 방식으로 변수, 함수, 타입, 인터페이스 등에 붙여 사용합니다.
```js
// types.ts
export interface Todo {
    title: string;
    checked: boolean;
}
```
<br>

### Import
- ES6의 `import`와 동일한 방식으로 사용합니다.
```js
// app.ts
import { Todo } from './types'

let item: Todo = {
    title: '할 일 1',
    checked: false
}
```

<br>

## Tip
1. `import` 를 입력하고
2. `{}` 빈 객체를 먼저 만듭니다.
3. `from './경로'` 부터 잡는 것이 `{}` 안에서 몇 글자만 입력해도 자동완성 됩니다.

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/usage/modules.html#%EC%86%8C%EA%B0%9C)