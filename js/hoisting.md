# Hoisting(호이스팅)

자바스크립트 엔진이 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미합니다. <br>

즉, 코드가 실행하기 전 `변수선언/함수선언` 이 해당 스코프의 최상단으로 끌어 올려지는 것같은 현상을 말합니다.

- 호이스팅은 스코드 단위로 발생 합니다.

<br>

## environmentRecord 과 hoisting의 연관
- environmentRecord 는 실행 컨텍스트 객체는 활성화되는 시점에 VariableEnvironment, LexicalEnviornment 환경에 속합니다.

- environmentRecord 는 변수명만 끌어올리고 할당 과정은 그대로 남겨두게 됩니다.

- environmentRecord 는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장 됩니다.
    - 매개변수 식별자
    - 함수 자체
    - 함수 내부의 식별자

<br>

## hoisting 과 var, let, const 
```js
// 호이스팅 때문에 선언이 끌어올려져서 오류 안남.
console.log(text); // (선언 + 초기화 된 상태)
text = 'Hanamon!'; // (선언 + 초기화 + 할당 된 상태)
var text;
console.log(text);

// 호이스팅 때문에 선언이 끌어올려졌지만 초기화 안된 상태에서 참조해서 오류 남.
console.log(text); // (선언 된 상태, 초기화(메모리 공간 확보와 undefined로 초기화) 안되서 참조 불가능 -> 에러남)
let text; // 여기서 초기화 단계가 실행됨


const text; // 에러남. 주의! 애초에 const 키워드로 재할당 불가능! 그래서 선언과 동시에 할당해야함
```
- var는 선언하기 전에 사용할 수 있습니다.
- let, const 모두 호이스팅이 일어납니다. -> 하지만 이 둘은 TDZ(Temporal Dead Zone)에 영향을 받습니다.
- TDZ 영역에 있는 변수들은 사용할 수 없습니다. -> 할당을 하기 전에는 사용할 수 없습니다.

```js
// 정상적인 코드
let age = 30;

function showAge(){
    console.log(age);
}

showAge();

// 문제가 발생하는 코드
let age = 30;

function showAge(){
    console.log(age); // TDZ 구역 : 스코프의 시작 지점부터 초기화 시작 지점까지의 구간

    let age = 20;
}

showAge();
```

<br>

## 변수의 생성과정과 연관 ?

### var
1. 선언 및 초기화 단계
2. 할당 단계

- var 키워드로 선언한 변수는 선언 단계와 초기화 단계가 한번에 이루어집니다.
- 즉, 변수를 등록(선언 단계)하고 메모리에 변수를 위한 공간을 확보한 후, undefined로 초기화 합니다. 

- 따라서 변수 선언문 이전에 변수에 접근하여도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않습니다.

<br>

### let
1. 선언 단계
2. 초기화 단계
3. 할당 단계

- let 키워드로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행 됩니다.
- 변수를 등록(선언 단계)하지만 초기화 단계는 변수 선언문에 도달했을 때(코드 실행 후) 이루어집니다. 
- 초기화 이전에 변수에 접근하려고 하면 참조 에러가 발생 합니다. -> 이는 아직 변수가 초기화되지 않았기 때문이다. 
- 즉, 변수를 위한 메모리 공간이 아직 확보되지 않았기 때문입니다. 
- 따라서 스코프의 시작 지점부터 초기화 시작 지점까지는 변수를 참조할 수 없다. 

- 스코프의 시작 지점부터 초기화 시작 지점까지의 구간을 ‘일시적 사각지대(Temporal Dead Zone; TDZ)’라고 부릅니다.

<br>

## 함수(function) 또한 호이스팅이 일어난다.
```js
foo1(); // 함수 선언문에서는 호이스팅 일어난다.
foo2(); // 함수 표현식이라서 호이스팅 안된다.
function foo1() {
  console.log('Hello');
}
var foo2 = function() {
  console.log('world');
}
```



<br>

## 참고
- [코딩앙마](https://www.youtube.com/watch?v=ocGc-AmWSnQ&list=PLZKTXPmaJk8JZ2NAC538UzhY_UNqMdZB4)
- [호이스팅이란](https://hanamon.kr/javascript-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85%EC%9D%B4%EB%9E%80-hoisting/)
- [실행_컨텍스트_구성](https://junilhwang.github.io/TIL/Javascript/Domain/Execution-Context/#_2-%E1%84%89%E1%85%B5%E1%86%AF%E1%84%92%E1%85%A2%E1%86%BC-%E1%84%8F%E1%85%A5%E1%86%AB%E1%84%90%E1%85%A6%E1%86%A8%E1%84%89%E1%85%B3%E1%84%90%E1%85%B3-%E1%84%80%E1%85%AE%E1%84%89%E1%85%A5%E1%86%BC)
- https://taenami.tistory.com/109
