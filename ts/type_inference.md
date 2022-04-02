# 타입 추론(Type Inference)
타입 추론이란 타입스크립트가 코드를 해석해 나가는 동작을 의미합니다.

<br>

## 타입 추론의 기본
```js
let x = 3;
```
- 위와 같이 `x`에 대한 타입을 따로 지정하지 않더라도 일단 `x`는 `number`로 간주됩니다.

- 이렇게 __변수를 선언하거나 초기화 할 때 타입이 추론됩니다.__
-  이외에도 변수, 속성, 인자의 기본 값, 함수의 반환 값 등을 설정할 때 타입 추론이 일어납니다.

<br>

## 인터페이스와 제네릭을 이용한 타입 추론
```js
interface Dropdown<T>{
    value: T;
    title: string;
}

let shoppingItem: Dropdown<string> = {
    value: 'abc',
    title: 'hello'
}
```
- 제네릭을 이용해 `string` 을 넘겼기 때문에, `value` 의 타입은 `string`이 됩니다.

<br>

```js
interface Dropdown<T>{
    value: T;
    title: string;
}

interface DetailedDropdown<T> extends Dropdown<T> {
    description: string;
    tag : T;
    // 암묵적으로 value, title 들어온다.
}

let detailedItem: DetailedDropdown<string> = {
    title: 'abc',
    description: 'ab',
    value: 'a', // string이라고 정의. 
    tag: 'b'
}

```
![bandicam 2022-04-02 17-44-16-771](https://user-images.githubusercontent.com/87301268/161375168-96d74143-f843-4887-a230-67eb7a9bb985.jpg)

<br>

## 가장 적절한 타입(Best Common Type)
```js
let arr = [1,2,3]; // number[]
let arr1 = [1,2,true] // (number | boolean)[]
```
- 가장 근접한 타입을 추론 하는 뜻 입니다. ( 모든 값 을 유니온으로 묶어나간다. )

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/type-inference.html#%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0-type-inference)