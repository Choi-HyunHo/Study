# var, let, const

자바스크립트의 변수 및 상수 선언 키워드 입니다.

## var
-> 초기 자바스크립트는 변수 선언을 `var` 만을 사용 했습니다.

<br>

### var 특징
- 함수 레벨 스코프(Function Level Scope) 또는 전역 스코프
```js
var a = "a";

function example() {
  var b = "b";
  console.log(a); // a 전역변수. 출력가능.

  if (true) {
    var c = "c";
    console.log(b); // b - 해당 함수 내 선언한 변수. 출력 가능.
  }
  console.log(c); // c - 해당 함수 내 선언한 변수. 출력 가능.
}
example();
```
- var는 변수의 중복 선언 가능
```js
var name = "kim";
console.log(name); // kim

var name = "lee";
console.log(name); //lee
```
- 값의 재할당이 가능
```js
var a = 10; 
a = 20; 
console.log(a); // 20
```
- 선언하기 전에 사용이 가능
```js
function sayHi() {
  phrase = "Hello";

  alert(phrase);
  var phrase;
}
sayHi();
```
<br>

## 하지만 var 는 문제점이 많기 때문에 사용을 자제 합니다.
- 대부분의 var의 특징이 좋지 않은 부작용을 가져옵니다.
- 초창기에는 유연성을 이용하여 프로그램을 작성 하였지만, 어느 정도이 규모있는 프로젝트를 하다 보면<br> 선언하지 않는 값들이 할당 되어져 있을 수도 있고 위험 부담이 큽니다.

<br>

## let
-> ES6(ES2015)부터 let과 const가 추가 되었습니다.

<br>

### let 특징
- 중복 선언 불가능
```js 
let a = 10; 
let a = 20; // SyntaxError: Identifier 'a' has already been declared
```

- 값의 재할당 가능
```js
let b = 111; 
b = 222; 
console.log(b); // 222
```

- let 은 함수 내부는 물론, if문이나 for문 등의 코드 블럭{ ... } 에서 선언된 변수도 지역변수로 취급 합니다. (`블럭 레벨 스코프(Block Level Scope)`)
```js
let a = "a";

function example() {
  let b = "b";
  console.log(a); // a 전역변수. 출력 가능

  if (true) {
    let c = "c";
    console.log(b); // b 해당 함수 내 선언한 변수. 출력 가능
  }
  console.log(c); // ReferenceError: c is not defined
}
example();
```

<br>

## const
-> 한 번 값을 선언하면 절대 바뀌지 않습니다. (상수)

<br>

### const 특징

- 중복 선언 불가능
```js
const b = 10; 
const b = 20; // SyntaxError: Identifier 'b' has already been declared
```
-  값의 재할당이 '불가능'
```js
const c = 111; 
c = 222; // TypeError: Assignment to constant variable.
```
- const는 함수 내부는 물론, if문이나 for문 등의 코드 블럭{ ... } 에서 선언된 변수도 지역변수로 취급 합니다. (`블럭 레벨 스코프(Block Level Scope)`)

<br>

## 정리

- 값 변경 가능 유무 
    - var과 let은 값이 선언된 이후에 값을 변경할 수 있지만, const는 생성할 때 선언된 초기값을 변경할 수 없습니다. 
- 함수스코프 vs 블록스코프
    - var은 함수 스코프를 가지지만, let과 const 변수는 블록 스코프를 가집니다. 
- 중복 선언은 var만 가능 합니다.

<br>

## 참고
- [드림코딩](https://www.youtube.com/watch?v=OCCpGh4ujb8&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=3)
- https://ko.javascript.info/var
- https://wonyoung2257.tistory.com/27
- https://curryyou.tistory.com/192