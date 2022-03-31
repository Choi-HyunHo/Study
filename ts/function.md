# 타입스크립트의 함수

## 함수의 `파라미터(매개변수)` 타입
```js
function sum(a: number, b: number){
    return a + b;
}

sum(10,20);
```

<br>

## 함수의 `반환 값` 에 타입을 정의하는 방식
```js
function add(): number{
    return 10;
}
```

<br>

## 함수의 기본적인 타입 선언
```js
// JS
function sum(a, b) {
  return a + b;
}

// TS
function sum(a: number, b: number): number {
  return a + b;
}
```

<br>


## 함수의 `옵셔널 파라미터` (선택적 파라미터)
- 타입스크립트에서는 함수의 인자를 모두 필수 값으로 간주합니다.

- 즉, 정의된 매개변수 값만 받을 수 있고 추가로 인자를 받을 수 없다는 의미입니다.
- 하지만 __필요에 따라서 선택적으로 사용하고 싶은 인자__ 앞에 `?` 을 사용 합니다.
```js
function log(a: string, b?: string){

}
log('hello world'); // b에 ?을 뺴면 2개를 넘겨야 되는데 하나만 넘겼다고 에러가 뜬다.
log('hello ts', 'abc')

function sum(a: number, b?: number): number {
  return a + b;
}
sum(10, 20); // 30
sum(10, 20, 30); // error, too many parameters
sum(10); // 10
```


<br>

## 자바스크립트와 다른 점

### 자바스크립트는
```js
function sum(a,b) {
    return a + b;
}

// 10과 20은 각각 a와 b에 들어갑니다.
// 추가적으로 나머지 인자들에 대해서는 반응을 하지 않습니다. <= 자바스크립트의 유연함
sum(10, 20, 30, 40);
```

<br>

### 타입스크립트는
```js
function sum(a: number, b: number): number{
    return a + b;
}

sum(10, 20, 30, 40); // 2개의 인수가 필요한데 4개를 가져왔다고 에러가 발생합니다.
```

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/basic-types.html#any)