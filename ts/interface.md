# Interface ( 인터페이스 )

인터페이스는 상호 간에 정의한 약속 혹은 규칙을 의미합니다.<br>

타입스크립트에서 반복되는 타입들을 모아서 하나의 인터페이스로 정의를 하여 사용 합니다.<br>

<br>

## 변수에 활용한 인터페이스
- 사용할 때 반드시 인터페이스에 있는 속성을 정의해야 합니다.
```js
// 인터페이스 선언
interface User {
    age: number;
    name: string;
}

let hyunho: User = {
    age: 26,
    name: '현호'
}
```

<br>

## 함수에 활용한 인터페이스
- 인자에 인터페이스를 적용 -> 특정 형식을 준수하는 데이터만 받겠다고 정의 합니다.
```js
interface User {
    age: number;
    name: string;
}

function getUser(user: User){
    console.log(user);
}

const capt = {
    name: 'captin',
    //age: 100
}

getUser(capt); // 오류 발생, capt 은 속성에 name 만 가지고 있고, age는 없기 때문에
```

<br>

## 함수의 구조에 인터페이스 활용
```js
interface sumFuntion{
    (a: number, b: number): number;
}

let sum: sumFuntion;

sum = function(a: number, b: number): number{
    return a + b;
}
```

<br>

## 인덱싱
```js
// 인덱싱
// 배열의 인덱스는 항상 숫자. 0 부터 시작
interface StringArray{
    [index: number]: string; // index 는 기존처럼 number 을받고, : 옆에 타입을 지정 할 수 있다. 
}

let arr: StringArray = ['a','b','c'];
arr[0] = 10 // 오류
// index를 이용해서 number 로 접근을 하는 것은 맞다.
// 하지만 배열안에 들어간 모든 타입은 string으로 선언되었기 때문에 오류가 난다.
```

<br>

## 인터페이스 확장
- `extends` 키워드 사용
```js
// 인터페이스 확장
// extends 키워드 사용, 
interface Person {
    name: string;
    age: number;
}

// 아래와 같은 인터페이스가 있다면, 중복되는 값들을 다른 인터페이스가 가지고 있는 경우
// interface Developer {
//    name: string;
//    age: number;
//    language: string;
//}

interface Developer extends Person {
    language: string;
}

// 오답
let john: Developer = {
    language: 'ts', // 에러 발생. 
    // developer 를 person으로 확장을 받았기 때문에 person에 있는 타입들 까지 정의를 해주는 것이 규칙
    // age: 20,
    // name: '존'
}

// 정답
let john: Developer = {
    language: 'ts'
    age: 20,
    name: '존'
}
```

<br>

## 옵션 속성
- 인터페이스를 사용할 때 인터페이스에 정의되어 있는 속성을 모두 다 꼭 사용하지 않아도 됩니다.
```js
interface 인터페이스_이름 {
  속성?: 타입;
}
```
```js
interface CraftBeer {
  name: string;
  hop?: number;  
}

let myBeer = {
  name: 'Saporo'
};
function brewBeer(beer: CraftBeer) {
  console.log(beer.name); // Saporo
}
brewBeer(myBeer);
```
<br>

## 읽기 전용 속성
- 인터페이스로 객체를 처음 생성할 때만 값을 할당하고 그 이후에는 변경할 수 없는 속성을 의미합니다.
- `readonly` 키워드 사용
```js
interface CraftBeer {
  readonly brand: string;
}

let myBeer: CraftBeer = {
  brand: 'Belgian Monk'
};
// 인터페이스로 객체를 선언하고 나서 수정하려고 하면 아래와 같이 오류가 납니다.
myBeer.brand = 'Korean Carpenter'; // error!
```

<br>

## 참고
- https://poiemaweb.com/typescript-interface
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/interfaces.html#%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)