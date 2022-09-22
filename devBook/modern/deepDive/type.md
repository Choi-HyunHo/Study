# 09 타입 변환과 단축 평가

## 9.1 타입 변환 ?
자바스크립트의 모든 값은 타입을 가지는데,
- 개발자의 의도에 따라 값의 타입을 변환하는 것을
-> __명시적 타입 변환__  또는 __타입 캐스팅__ 이라고 한다.

- 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 __암묵적으롤 타입이 자동 변환되는 것을__
-> __암묵적 타입 변환__ 또는 __타입 강제 변환__ 이라고 한다.

<br>

> __자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적으로 타입 변환을 통해 표현식을 평가__

<br>

- 그렇다고 명시적 타입 변환이나 암묵적 타입 변환이나 기존 원시값을 직접 변경하는 것은 아니다.
- 원시 값은 __변경 불가능한 값__ 이다.

<br>

즉, 타입변환이란
- 기존 원시 값을 사용해 다른 타입의 __새로운 값을 생성__ 하는 것

***
<br>

## 9.2 암묵적 타입 변환
> 자바스크립트 엔진이 표현식을 평가할 때, __개발자의 의도와 상관없이__ 코드의 문맥을 고려해 __암묵적으로 데이터 타입을 강제 변환__ 하는 것

<br>

### 9.2.1 문자열 타입으로 변환
- `+` 또는 `템플릿 리터럴`
- 템플릿 리터럴은 표현식의 평가 결과를 문자열 타입으로 암묵적 변환
- __문자열 연결 연산자는 코드의 문맥상 모두 문자열 타입__ 이어야 한다.

<br>

> 자바스크립트 엔진은 문자열 연결 연산자를 만나면, <br> __문자열 타입이 아닌 값을 문자열 타입으로__ 암묵적 타입 변환하여 수행

<br>

```js
// 숫자 타입
0 + ''              // "0"
  -0 + ''             // "0"
1+ ''               // "1"
  -1 + ''             // "-1"
NaN + ''            // "NaN"
Infinity + ''       // "Infinity"
  -Infinity + ''      // "-Infinity"

// boolean 타입
true + ''           // "true"
false + ''          // "false"

// null 타입
null + ''           // "null"

// undefined 타입
undefined + ''      // "undefined"

// 심벌 타입
(Symbol()) + ''     // "TypeError"

// 객체 타입
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[10, 20] + ''       // "10, 20"
(function(){}) + '' // "function()"
Array + ''          // "function Array() {[native code]}"
```

<br>

### 9.2.2 숫자 타입으로 변환
- +, -, / 는 모두 산술 연산자
- __산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입__ 이어야 한다.
- 자바스크립트 엔진은 __+__ 단항 연산자를 만나면
- 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행

<br>

### 자바스크립트 엔진은 산술 연산자를 만나면,
- __숫자 타입이 아닌 값을 숫자 타입으로__ 암묵적 타입 변환하여 동작
- 이 때, 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 결과는 __NaN__ 이 된다.

<br>

```js
// 숫자 타입
+''               // 0
+'0'              // 0
+'1'              // 1 
+'string'         //  NaN

// boolean 타입
+true             //  1
+false            //  0

// null 타입
+null             //  0

// undefined 타입
+undefined        //  NaN

// 심벌 타입
+Symbol()         //  TypeError

// 객체 타입
+{}               //  NaN
+[]               //  0
+[10, 20]         //  NaN
+(function(){})   //  NaN
```

<br>

#### 비교 연산자
```js 
'1' > 0 // true
```
- 비교 연산자는 피연산자의 크기를 비교하므로, __코드의 문맥상 모두 숫자 타입__ 이어야 한다.

<br>

### 9.2.3 boolean 타입으로 변환
- if 문과 for 문과 같은 제어문 또는 삼항 조건 연산자의 조건식은
-> __논리적 참 / 거짓 으로 평가되어야 하는 표현식__ 으로
-> 조건식의 평과 결과를 __불리언 타입으로 암묵적 타입 변환을 한다.__

- 자바스크립트 엔진은 불리언 타입이 아닌 값을
-> Truthy 값 또는 Falsy 값으로 구분해 각각 true와 false로 암묵적 타입 변환 한다.
-> Truthy 값 또는 Falsy 값 앞에 느낌표(!)를 붙어주면 각각 true와 false로 전달된다.

- false로 평가되는 Falsy 값
-> false
-> undefined
-> null
-> 0, -0
-> NaN
-> ‘’(빈 문자열)

***
<br>

## 9.3 명시적 타입 변환

### 9.3.1 문자열 타입으로 변환
#### 1. `String()` 생성자 함수 사용
```js
// 숫자 타입 => 문자열 타입
String(1);              // "1"
String(NaN);            // "NaN"
String(Infinity);       // "Infinity"

// 불리언 타입 ➔ 문자열 타입
String(true)            // "true"
String(false)           // "false"
```

<br>

#### 2. Object.prototype.toString 메서드를 사용하는 방법
```js
// 숫자 타입 ➔ 문자열 타입
(1).toString();         // "1"
(NaN).toString();       // "NaN"
(Infinity)toString();   // "Infinity"

// 불리언 타입 ➔ 문자열 타입
(true).toString();      // "true"
(false).toString();     // "false"
```

<br>

#### 3. 문자열 연결 연산자를 이용
```js
// 숫자 타입 ➔ 문자열 타입
1 + ''                  // "1"
NaN + ''                //  "NaN"
Infinity + ''           // "Infinity"

// 불리언 타입 ➔ 문자열 타입
true + ''               // "true"
false + ''              // "false"
```

<br>

### 9.3.2 숫자 타입으로 변환
#### 1. Number() 생성자 함수를 사용
```js
// 문자열 타입 ➔ 숫자 타입
Number('0');     // 0
Number('-1');    // -1
Number('10.53'); // 10.53

// 불리언 타입 ➔ 숫자 타입
Number(true);    // 1
Number(false);   // 0
```

<br>

#### 2. parseInt, parseFloat 함수 사용 (문자열 만)
```js
parseInt('0');       // 0
parseInt('-1');      // -1
parseFloat('10.53'); // 10.53
```

<br>

#### 3. + 단항 산술 연산자를 사용
```js
// 문자열 타입 ➔ 숫자 타입
+'0';     // 0
+'-1';    // -1
+'10.53'; // 10.53

// 불리언 타입 ➔ 숫자 타입
+true;    // 1
+false;   // 0
```

#### 4. * 산술 연산자 사용
```js
// 문자열 타입 ➔ 숫자 타입
'0' * 1;     // 0
'-1' * 1;    // -1
'10.53' * 1; // 10.53

// 불리언 타입 ➔ 숫자 타입
true * 1;    // 1
false * 1;   // 0
```

<br>

### 9.3.3 boolean 타입으로 변환
#### 1. Boolean() 사용
```js
// 문자열 타입 ➔ 불리언 타입
Boolean('x');       // true
Boolean('');        // false
Boolean('false');   // true

// 숫자 타입 ➔ 불리언 타입
Boolean(0);         // false
Boolean(1);         // true
Boolean(NaN);       // false
Boolean(Infinity);  // true

// null 타입 ➔ 불리언 타입
Boolean(null);      // false

// undefined 타입 ➔ 불리언 타입
Boolean(undefined); // false

// 객체 타입 ➔ 불리언 타입
Boolean({});        // true
Boolean([]);        // true
```

<br>

#### 2. ! 부정 논리 연산자를 두 번 사용하는 방법
```js
// 문자열 타입 ➔ 불리언 타입
!!'x';       // true
!!'';        // false
!!'false';   // true

// 숫자 타입 ➔ 불리언 타입
!!0;         // false
!!1;         // true
!!NaN;       // false
!!Infinity;  // true

// null 타입 ➔ 불리언 타입
!!null;      // false

// undefined 타입 ➔ 불리언 타입
!!undefined; // false

// 객체 타입 ➔ 불리언 타입
!!{};        // true
!![];        // true
```

***

## 9.4 단축 평가
__타입 변환을 하지 않고 그대로 반환__ 하는데 이를 __단축 평가__ 라고 한다.

- 단축 평가는 표현식을 평가하는 도중에 평가결과가 확정된 경우
-> __나머지 결과를 생략__
-> 단축평가를 사용하면 if문 대체
-> 삼항 조건 연산자는 __if-else__ 문을 대체

```js
단축 평가 표현식	평과 결과
true ¦¦ anything	true
false ¦¦ anything	anything
true && anything	true
false && anything	false
```

<br>

### 9.4.1 논리 연산자를 사용한 단축 평가

> 논리합 || 또는 논리곱&&.

<br>

- 표현식의 평가 결과는 불리언 값이 아닐 수도 있다
- __논리합||__ 또는 __논리곱&&__. 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다
- __논리곱&& 연산자__ 는 두 개의 피연산자가 __모두 true로 평가될 때 true__ 를 반환.
- 좌항에서 우항으로 평가가 진행.
- __논리합|| 연산자는__
- 두 개의 피연산자 중 __하나만 true로 평가되어도 true__ 를 반환.

```js
'cat' || 'dog'   // cat
false || 'dog'   // dog
'cat' || false   // cat

'cat' && 'dog'   // dog
false && 'dog'   // false
'cat' && false   // false
```