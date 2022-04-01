# Type Aliases ( 타입 별칭 )
타입 별칭은 특정 타입이나 인터페이스를 참조할 수 있는 타입 변수를 의미합니다. 

<br>

## 사용하는 방법
```js
// string 타입을 사용할 때
const name: string = 'capt';

// 타입 별칭을 사용할 때
type MyName = string;
const name: MyName = 'capt';
```

<br>

## 타입 별칭의 특징 ( 인터페이스와 비교 )
```js

interface Person{
    name: string;
    age: number;
}


type Person = {
    name: string;
    age: number;

}


let seho: Person ={
    name: '세호',
    age: 30
}
```
<img src="https://user-images.githubusercontent.com/87301268/161200186-e724d630-a40a-4371-b884-11bc9af5b1ca.jpg" align = left>


![bandicam 2022-04-01 14-02-53-281](https://user-images.githubusercontent.com/87301268/161200254-8da60012-6e38-41f6-9708-182d80f6893a.jpg)


- 타입 별칭은 새로운 타입 값을 하나 생성하는 것이 아니라 정의한 타입에 대해<br> 나중에 쉽게 참고할 수 있게 이름을 부여하는 것과 같습니다.

<br>

## Type vs Interface
- 타입 별칭과 인터페이스의 가장 큰 차이점은 타입의 `확장 가능 / 불가능 여부` 입니다.<br> 인터페이스는 확장이 가능한데 반해 타입 별칭은 확장이 불가능합니다.

- 가능한 확장이 가능한 `Interface` 를 사용 하는 것을 추천 합니다.

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/type-alias.html#%ED%83%80%EC%9E%85-%EB%B3%84%EC%B9%AD-type-aliases)