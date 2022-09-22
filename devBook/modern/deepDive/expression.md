# 05_표현식과 문
## 5.1 값(value)
> <span style="color:#0000FF">값(value)</span>은 __식(표현식 expression)__ 이 __평가(evaluate)__ 되어 생성된 결과를 말한다.
```js
// 10 + 20 은 평가되어 숫자 30을 생성한다.
10 + 20; // 30
```

모든 값은 데이터 타입을 가지며, 메모리에 2진수, 즉 비트(bit)의 나열로 저장된다.
- 메모리에 저장된 값은 __데이터 타입에 따라 다르게 해석__ 될 수 있다.
- ex) 메모리에 저장된 값 0100 0001을 __숫자로 해석하면 65__ 지만, __문자로 해석하면 A다.__

<br>

#### 변수 ? 
하나의 값을 저장하기 위해 확보한 메모리 공간 자체 or 그 메모리 공간을 식별하기 위해 붙인 이름 
-> 변수에 할당되는 것은 <span style="color : #0000FF">값</span>이다.

***
<br>

## 5.2 리터럴(literal)
<span style="color:#0000FF">리터럴(literal)</span>은 사람이 이해할 수 있는 문자 또는 __약속된 기호를 사용해 값을 생성하는 표기법(notation)__ 을 말한다.

> __리터럴__ 은 사람이 이해할 수 있는 문자(아라비아 숫자, 알파벳, 한글 등) 또는
미리 약속된 기호('', "", ., [], {}, // 등)로 표기한 코드.
리터럴이 __값으로 평가__ 된다면, 리터럴도 __표현식__ 이다.

|리터럴|예시|
|:--|:--|
|정수 리터럴|	100	
|부동소수점 리터럴|	10.5	
|2진수 리터럴|	0b01000001
|8진수 리터럴|	0o101	
|16진수 리터럴|	0x41	
|문자열 리터럴|	'hello' "world"	
|불리언 리터럴|	true false	
|null 리터럴|	null	
|undefined 리터럴|	undefined	
|객체 리터럴|	{ name: 'april', address: 'Seoul' }	
|배열 리터럴|	[ 1, 2, 3 ]	
|함수 리터럴|	function() {}	
|정규표현식 리터럴|	/[A-Z]+/g

***
<br>

## 5.3 표현식(expression)
><span style="color:#0000FF">표현식</span>은 __값__ 으로 평가될 수 있는 모든 <span style="color:green">문(statement)</span>이다.
표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다.

<br>

- 예제 1
```js
const score = 100;
```
__100__ 은 리터럴이지만, 
자바스크립트 엔진에 의해 평가되어 __값을 생성__ 하므로 __리티럴은 그 자체로 표현식__ 이다.

<br>

- 예제 2
```js
const score = 50 + 50;
```
__50 + 50__ 은 리터럴과 연산자로 이루어져 있다.
하지만, __50 + 50__ 마찬가지로 평가되어 숫자 값 100 을 생성하므로 표현식이다.

<br>

- 예제 3
```js
score;
```
변수 __score__ 를 참조하면 변수 값으로 평가된다.
__식별자 참조__ 는 값을 생성하지는 않지만, __값으로 평가__ 되므로 표현식이다.

<br>

- 정리

```js
// 리터럴 표현식
10
'Hello'

// 식별자 표현식(선언이 이미 존재한다고 가정)
sum
person.name
arr[1]

// 연산자 표현식
10 + 20 
sum = 10
sum !== 10

// 함수/메서드 호출 표현식 (선언은 이미 존재한다고 가정)
square()
person.getName()
```


> 표현식은 리터럴, 식별자, 연산자, 함수 호출 등의 조합으로 이뤄질 수 있다.
다양한 표현식이 있지만 값으로 평가된다는 점은 모두 동일하다.

***
<br>

## 5.4 문(statement)
> <span style="color:green">문(statement)</span>은 __프로그램을 구성하는 기본 단위__ 이자 __최소 실행 단위__

<br>

__문의 집합__ 으로 이뤄진 것이 __프로그램__ 이며,
문을 작성하고 __순서에 맞게 나열__ 하는 것이 __프로그래밍__ 이다.

문은 여러 __토큰(token)__ 으로 구성되는데, 토큰이란 __문법적인 의미를 가지며__
문법적으로 __더 이상 나눌 수 없는 코드의 기본 요소__ 를 의미한다.
![](https://velog.velcdn.com/images/hoho_0815/post/e545747d-c842-4828-9f94-dbfbbdef2502/image.png)

- 문을 명령문이라고도 부르는데, 컴퓨터에 내리는 명령이기 때문

***
<br>

## 5.5 세미콜론(;)
> __세미콜론(;)__ 은 __문의 종료__ 를 의미하며 옵션이라 생략 가능 하다.

- 자바스크립트 엔진은 세미콜론으로 문의 종료한 위치를 파악, 순차적으로 하나씩 문을 실행한다.
- 단, __코드 블록({ ... })__ 뒤에는 세미콜론을 붙이지 않는데,
코드 블록은 언제나 문의 종료를 의미하는 __자체 종결성(self closing)__ 을 갖기 때문.

***
<br>

## 5.6 표현식인 문과 표현식이 아닌 문
>__표현식인 문__ : 값으로 평가될 수 있는 문이며, <br>
__표현식이 아닌 문__: 값으로 평가될 수 없는 문을 말한다

<br>

```js
var x; // 변수 선언문은 값으로 평가될 수 없으므로 표현식 X  

x = 100; // 표현식 O

var foo = 10; // 표현식 X

if (true) {} // 값으로 평가될 수 없으므로 표현식 X


// 변수에 할당해보기!
var foo = var x; // 표현식이 아닌 문은 값처럼 사용할 수 없다.

var foo = x = 100; // 표현식인 문은 값처럼 사용할 수 있다.
```