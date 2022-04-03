# 타입 호환(Type Compatibility)
타입스크립트 코드에서 특정 타입이 다른 타입에 잘 맞는지를 의미합니다. 

<br>

## 구조적 타이핑 예시
- 구조적 타이핑(structural typing)이란 코드 구조 관점에서 타입이 서로 호환되는지의 여부를 판단하는 것입니다. 

<br>

### 인터페이스
```js
interface Developer{
    name: string;
    skill: string;
}

interface Person{
    name: string;
}

let developer: Developer;
let person: Person;

developer = person; // 에러이유 : 왼쪽에 있는 변수가 더 큰 관계를 가지고 있어서
person = developer; // 에러가 안뜬다.
```
- 오른쪽에 있는 타입이 더 많은 속성을 가지거나, 구조적으로 더 컸을 때 왼쪽과 호환이 됩니다.

<br>

### 인터페이스와 클래스
```js
interface Developer{
    name: string;
    skill: string;
}

class Person{
    name: string;
}

let developer: Developer;

developer = new Person(); // 에러 발생
```
- 인터페이스나 클래스, 타입별칭 과 같은 것으로 확인하지 않고 
- __내부적으로 존재하고 있는 속성과 티입에 대한 정의들에 대해 비교를 합니다.__

<br>

## 함수 비교
```js
let add = function(a: number){
    // ...
}

let sum = function(a: number, b: number){
    // ...
}

add = sum; // 에러
sum = add // 가능
```
- 위의 함수의 차이는 파리미터의 개수, 구조적으로는 `sum` 이 `add` 보다 큽니다.

<br>

## 제네릭 비교
1.
```js
interface Empty<T> {
    // ...
}

let empty1: Empty<string>;
let empty2: Empty<number>;

// 아래 모두 호환 가능
empty1 = empty2; 
empty2 = empty1;
```
- 위 인터페이스는 일단 속성이 없기 때문에 같은 타입으로 간주됩니다. 

<br>

2.
```js
interface NotEmpty<T> {
    date: T;
}

let notempty1: NotEmpty<string>;
let notempty2: NotEmpty<number>;

// 모두 호환 불가능
// 동일한 속성을 가지지만 타입의 차이가 발생한다.
notempty1 = notempty2;
notempty2 = notempty1; 
```
- 인터페이스 NotEmpty에 넘긴 제네릭 타입<T>이 data 속성에 할당 되었으므로 서로 다른 타입으로 간주됩니다.

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/type-compatibility.html#generics)