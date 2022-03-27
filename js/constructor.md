# 생성자 함수

필요한 재료들을 넣어주고 찍어내는 붕어빵 판과 같습니다.

<br>

## 문법
```js
function User(name, age){ // 첫 글자는 대문자로
    this.name = name;
    this.age = age;
}

let user1 = new User("Mike", 30);
let user2 = new User("Jan", 17);
let user3 = new User("Tom", 20);
```
- `new` 연산자를 사용하여 호출

<br>

## 사용하는 이유
- 객체 리터럴 { ... } 을 사용하면 객체를 쉽게 만들 수 있습니다. 하지만 <br> 일일이 객체를 만드는 것보다 훨씬 빠르고 일관성 있게 만들 수 있습니다.

<br>

## 동작 원리
```js
function User(name, age){
    // this = {}; -> 빈 객체가 임시적으로 만들어짐

    // 새로운 프로퍼티을 this에 추가
    this.name = name;
    this.age = age;

    // return this; 
}

new 함수명();
```
1. new 함수명() 을 실행하면
2. this = {} 빈 객체를 만듭니다.
3. this에 프로퍼티들을 추가합니다. 

4. 마지막으로 return this 를 반환합니다.

- 2번과 4번은 코드 상에는 존재하지 않으나 위와 같은 알고리즘으로 동작 합니다.

<br>

## 참고
- [코딩앙마](https://www.youtube.com/watch?v=8hrSkOihmBI&list=PLZKTXPmaJk8JZ2NAC538UzhY_UNqMdZB4&index=2)
- https://ko.javascript.info/constructor-new