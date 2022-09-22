# 08 제어문

제어문은
- 조건에 따라 코드 블록을 실행 하거나(조건문)
- 반복 실행(반복문) 할 때 사용

코드는 위에서 아래로 순차적으로 실행하는데,
- 제어문을 사용하면 코드 실행 흐름을 인위적으로 제어할 수 있다. 하지만 직관적인 코드의 흐름을 혼란스럽게 만들어서 코드의 가독성을 해치는 단점이 있다.
- __forEach__, __map__, __filter__, __reduce__ 같은 함수형 프로그래밍 기법에서는 제어문의 사용을 억제하여 복잡성을 해결하고자 노력한다.

***
<br>

## 8.1 블록문
> 0개 이상의 문을 중괄호 `{}` 로 묶은 것으로 <span style="color:red"> 코드 블록 </span>이라고 부르기도 한다.

- 자바스크립트에서는 블록문을 __하나의 실행단위__ 로 취급한다.

```js
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

***
<br>

## 8.2 조건문
> <span style="color:red">주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정</span> 한다.

- 자바스크립트는 __if-else__ 문과 __switch__ 두가지 조건문을 제공한다.

<br>

### 8.2.1 if-else 문
- 주어진 조건식에 따른 __참과 거짓__(true / false) 으로 평가
- 조건식의 개수에 따라 코드 블록을 늘리고 __else if__ 문을 사용
- 대부분의 __if-else__ 문은 삼항 조건 연산자로 바꾸어 사용 할 수 있다.

<br>

```js
if (조건식) {
  // 조건식이 참이면 이 코드 블록이 실행
} else {
  // 조건식이 거짓이면 이 코드 블록이 실행
}

if (조건식1) {
  // 조건식1이 참이면 이 코드 블록이 실행
} else if (조건식2) {
  // 조건식2이 참이면 이 코드 블록이 실행
} else {
  // 조건식1, 2 모두 거짓이면 이 코드 블록이 실행
}
```

<br>

### 8.2.2 switch 문
- 주어진 표현식을 평가하여 __그 값과 일치하는__ 표현식을 가지는 __case__ 문으로 실행
- case 문은
1. 상황(case) 을 의미하는 표현식을 저장하고
2. 콜론(:) 으로 끝난다.
3. 그 뒤에 실행할 문들을 위치시킨다.
- __switch 문의 표현식과 일치하는 case 문이 없다면__
-> default 문으로 이동
-> default 문에서 __break__ 문을 생략하는 것이 일반적

<br>

```js
switch(표현식) {
  case 표현식1: 
    표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2: 
    표현식과 표현식2이 일치하면 실행될 문;
    break;
  case 표현식3: 
    표현식과 표현식3이 일치하면 실행될 문;
    break;
  default: 표현식과 일치하는 case문이 없을 때 실행될 문;
}
```
<br>

> __풀 스루(fall through) ?__ <br>
switch 문을 탈출하지 않고 switch 문이 끝날 때 까지 
모든 case 문과 default 문을 실행되는 것을 말한다.

<br>

- 풀 스루 예시
```js
// 월을 영어로 변환한다. (4 -> 'April')
var month = 4;
var monthName;

switch(month) {
    case 1: monthNAme = 'January';
    case 2: monthNAme = 'February';
    case 3: monthNAme = 'March';
    case 4: monthNAme = 'April';
    case 5: monthNAme = 'May';
    case 6: monthNAme = 'June';
    case 7: monthNAme = 'July';
    case 8: monthNAme = 'August';
    case 9: monthNAme = 'September';
    case 10: monthNAme = 'October';
    case 11: monthNAme = 'November';
    case 12: monthNAme = 'December';
    default: monthName = "Invalid month"
}
console.log(monthName);  // Invaild month
```

***
<br>

## 8.3 반복문
반복문은
- __조건식의 평가 결과가 참인 경우 코드 블록을 실행__
- 자바스크립트는 __for__, __while__, __do-while__ 문을 제공

<br>

### 8.3.1 for 문
> __조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행__

- for 문의 변수 선언문, 조건식, 증감식은 모두 옵션이므로 반드시 사용할 필요는 없다.

```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
  // 조건식이 참인 경우 반복 실행할 문;
}
```

![](https://velog.velcdn.com/images/hoho_0815/post/42e9ad4c-5542-4a7a-befb-9df67fe39719/image.png)

<br>

### 8.3.2 while 문
> 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속 실행하며 반복
- __for__ 문은 반복 횟수가 명확할 때 주로 사용
- __while__ 문은 __반복 회수가 불명확할 때 주로 사용__

<br>

```js
while(조건식) {
  // 조건식이 참일 때 실행할 문;
}

// 무한 루프
while(true) {
  // ...
}
```

- 무한 루프를 탈출하기 위해서는 
- 코드 블록 내의 __if__ 문으로 탈출 조건을 만들고,  __break__ 문으로 코드 블록을 탈출

<br>

### 8.3.3 do-while 문
> __코드 블록을 먼저 실행하고 조건식을 평가__

- 따라서 코드 블록은 무조건 한 번 이상 실행

```js
do {
  //  ...
} while (조건식);
```

<br>

### 8.3.4 break 문
> 주 사용은 __반복문__ 또는 __switch__ 문의 코드 블록을 탈출 할 때 사용

<br>

### 8.3.5 continue 문
> __반복문의 코드 블록을 현 지점에서 중단하고 증감식으로 실행 흐름을 이동__
- __break__ 문처럼 반복문을 탈출하지는 않는다.

<br>

```js
var string = 'Hello world';
var search = 'l';
var count = 0;

// 문자열은 유사 배열이므로 for 문으로 순회할 수 있다.
for(var i = 0; i < string.length; i++) {
  // 'l' 이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if(string[i] !== search) continue;
  count++;
}
console.log(count); // 3

// 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
const regexp = new RegExp(search, 'g');
console.log(string.match(regexp).length);  // 3
```
