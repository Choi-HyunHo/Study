# Union Type ( 유니온 타입 ) 과  Intersection Type 
파라미터 또는 변수에 하나의 타입 이상을 사용할 수 있게 해주는 것 입니다.

<br>

## 예시
```js
function logMessage(value: string){
    console.log(value)
}

logMessage('hello');
logMessage(100); // number 타입이 들어오기에 오류
// 숫자를 포함시키고 싶으면 string -> any 로 바꾸면 된다. <- 타입의 장점이 없어 집니다.
```
<br>

- 유니온 타입을 사용하면 ?

```js
function Message(value: string | number){
    console.log(value);
}

Message('hello');
Message(100) // 두 가지 타입 모두 받을 수 있습니다.
```
<br>

## 사용법
- `|` 키워드 사용

- ~ 이거나, 아래를 보면 셋 중에 하나
```js
let hyunho: string | number | boolean;
```

<br>

## 유니온 타입 장점
- 타입 가드 효과

>> 타입 가드 : 특정 타입으로 타입의 범위를 좁혀나가는(필터링 하는) 과정 

```js
function Message(value: string | number){
    if(typeof value === 'number'){
        value.toLocaleString 
        // 각 타입에 맞게 추론이 되기 떄문에 해당 타입의 API 를 사용할 수 있게 됩니다.
        // 타입 가드 : 특정 타입으로 타입의 범위를 좁혀나가는(필터링 하는) 과정
    }

    if(typeof value === 'string'){
        value.toString();
    }

    throw new TypeError('value must be string or number');
}

Message('hello');
Message(100)
```

<br>

## 유니온 타입 특징
```js
interface Developer{
    name: string;
    skill: string;
}

interface Person{
    name: string;
    age: number;
}

function askSomeone(someone: Developer | Person){
    someone.name
    // someone.skill 불가능
}
```
- Developer 와 Person 이 가지고 있는 모든 속성을 사용할 수 있다고 생각 할 수 있습니다. 
- 실제로는 `name` 만 사용할 수 있습니다.
- 이유는, someone 에 어떤 값이 들어올지 모르기 때문에 Developer 도 되야하고, Person 도 되어야 합니다.

- 즉, 타입스크립트에서 코드 상 에러가 발생할 수 있다고 생각하기 때문에,<br> __인터페이스나 특정 구조체를 유니온 타입으로 사용하면 보장된 속성만 제공 합니다.__

<br>

## Intersection Type 
- `&` 키워드 사용

- ~ 이면서, 아래를 보면 세 개의 타입을 전부 만족하는
```js
let capt: stirng & number & boolean;
```

<br>

## 인터섹션 타입 특징
```js
interface Developer{
    name: string;
    skill: string;
}

interface Person{
    name: string;
    age: number;
}

function askSomeone(someone: Developer & Person){
    someone.name;
    someone.skill;
    someone.age;
}
```
- Developer 와 Persone 의 속성을 모두 포함한 someone 입니다.

- 공통된 속성만 사용이 가능한 유니온 타입과의 차이점

<br>

## 유니온과 인터섹션의 차이점 ( 호출하는 관점 )
### Union Type
```js
function askSomeone(someone: Developer | Person){
}

askSomeone({name: '디벨로퍼', skill: '웹 개발'})
askSomeone({name: '캡틴', age: '100'})
```
- askSomeone 에 들어가는 인자로, Developer 나 Perosn 이기 때문에 둘 중 하나의 데이터를 주면 됩니다.

<br>

### Intersection Type 
```js
function askSomeone(someone: Developer & Person){  
}

// 에러
askSomeone({name: '디벨로퍼', skill: '웹 개발'})
// 이유는 Developer 의 속성과 Persone 의 속성을 모두 넘겨야 합니다.

// 아래와 같이 사용
askSomeone({name: '디벨로퍼', skill: '웹 개발', age: 34})
```

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/operator.html#union-type)