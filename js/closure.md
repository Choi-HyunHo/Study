# 클로저(closure)

내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것을 가리킵니다.

<br>

## 어휘적 환경(Lexical Environment)
```js
// (1)
// one : 초기화 X, addOne : function

let one; // (2) one : undefined, addOne : function
one = 1; // (3) one : 1, addOne : function -> 전역 Lexical

function addOne(num){
    console.log(one + num);
}

addOne(5); // (4) 함수가 실행되는 순간 새로운 Lexical 환경이 만들어집니다. num : 5
```
- (1) : 코드가 실행되면 스크립트 내에서 선언한 변수들이 `Lexical 환경` 에 올라갑니다.
     
- (4) 에서 함수가 넘겨받은 매개변수와 지역변수가 저장됩니다. <br>
함수가 호출되는 동안 함수에서 만들어진 `내부 Lexical 환경, 즉 (4)번`과 <br>
외부에서 받은 `전역 Lexical 환경(3)` 두 개를 가집니다.

- 내부 Lexical 환경은 외부 Lexical 환경에 대한 참조를 받습니다.
- 코드에서 변수를 찾을 때는 내부에서 찾고 없을 경우 외부, 그 다음 전역 Lexical 환경까지 범위를 넓혀서 찾습니다.

- 위의 코드에서 `one + num` 을 우선 내부 Lexical 환경에서 찾습니다.
- num 은 찾았지만 one 은 찾지 못했습니다.
- 찾지 못했으니 외부로 범위를 넓혀서 one 을 찾습니다.

<br>

## 예시 
```js
let base = 'Hello, ';
function sayHelloTo(name) {
  let text = base + name; // 외부 환경에서 찾는다.
  return function() {
    console.log(text); // 내부 환경에서 text라는 변수가 없기 떄문에
  };
}

var hello1 = sayHelloTo('승민');
var hello2 = sayHelloTo('현섭');
var hello3 = sayHelloTo('유근');
hello1(); // 'Hello, 승민'
hello2(); // 'Hello, 현섭'
hello3(); // 'Hello, 유근'
```
![캡처](https://user-images.githubusercontent.com/87301268/160283727-31b57508-dd0c-4142-8c08-c41b4bcf008d.JPG)

- 내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것을 가리킵니다.

<br>

## 클로저의 사용하는 이유
- 상태유지 : 현재 상태를 기억하고 변경된 최신 상태를 유지 할 수 있습니다.

- 정보의 은닉 : 자바스크립트에서 private 기능을 보완 합니다. <br>
클로저를 이용하면 제한적인 접근만을 허용하여 private한 변수 또는 메소드의 효과를 낼 수 있습니다.
>> 자바스크립트는 태생적으로 private, public 등의 별도 문법이 없는 언어


<br>

## 정리
- 클로저는 함수와 렉시컬 환경의 조합 입니다.
- 함수가 생성될 당시의 외부 변수를 기억하고 생성 이후에도 계속 접근이 가능 합니다.


<br>

## 참고
- [MDN, 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures)
- https://hanamon.kr/javascript-%ED%81%B4%EB%A1%9C%EC%A0%80/
- https://tylee82.tistory.com/316
- [왜, 클로저인가](https://velog.io/@mygomi/TIL-75-JS-Closure-%ED%81%B4%EB%A1%9C%EC%A0%80%EB%8A%94-%EC%99%9C-%ED%81%B4%EB%A1%9C%EC%A0%80%EC%9D%B8%EA%B0%80)
- [코딩앙마](https://www.youtube.com/watch?v=tpl2oXQkGZs&list=PLZKTXPmaJk8JZ2NAC538UzhY_UNqMdZB4&index=11)