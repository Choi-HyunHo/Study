# 타입스크립트 기본 타입

타입스크립트로 변수나 함수와 같은 자바스크립트 코드에 타입을 정의할 수 있습니다.

<br>

## 타입 표기(Type Annotation)
- `:` 를 이용하여 자바스크립트 코드에 타입을 정의하는 방식

<br>

## String
```js
// 기본 JS 선언
const str = 'hello';

// TS 문자열 선언
const str: string = 'hello';
```

<br>

## Number
```js
let num: number = 10;
```

<br>

## Array
```js
let arr: Array<number> = [1,2,3];
let heroes: Array<string> = ['captin','thor','hulk'] // 숫자를 넣으면 에러
let items : number[] = [1,2,3];
```

<br>

## Tuple
- 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식 입니다.
```js
let address: [string, number] = ['gangnam', 100];
```

<br>

## Object
```js
let obj: object = {};

let person: object = {
    name: 'capt',
    age: 100
};

let person: {name: string, age: number} = {
    name : 'thor',
    age : 1000
};

// object로 이루어진 배열
let todoItems: Todo[];
```

<br>

### 예시
```js
function fetchTodoItems(): object[] { // object로 이루어진 배열
  const todos = [
    { id: 1, title: '안녕', done: false },
    { id: 2, title: '타입', done: false },
	{ id: 3, title: '스크립트', done: false },
  ];
  return todos;
}

// 위의 코드를 아래와 같이 가능 (자세히 지정)
function fetchTodoItems(): { id: number; title: string; done: boolean }[] {
  const todos = [
    { id: 1, title: '안녕', done: false },
    { id: 2, title: '타입', done: false },
    { id: 3, title: '스크립트', done: false },
  ];
  return todos;
}
```

<br>

## Boolean
```js
let show: boolean = true;
```

<br>

## Any
- 단어 의미 그대로 모든 타입에 대해서 허용 합니다.
```js
let str: any = 'hi';
let num: any = 10;
let arr: any = ['a', 2, true];
```

<br>

## Void
-  반환값이 없습니다. ( 리턴값들이 없는 경우 )
```js
function deleteTodo(index: number): void {
  todoItems.splice(index, 1);
}

function log(): void {
  console.log(todoItems);
}
```

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/basic-types.html#any)