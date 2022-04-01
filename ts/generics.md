# 제네릭(Generics)
## 사전적 정의
제네릭은 C#, Java 등의 언어에서 재사용성이 높은 컴포넌트를 만들 때 자주 활용되는 특징입니다.<br>특히, 한가지 타입보다 여러 가지 타입에서 동작하는 컴포넌트를 생성하는데 사용됩니다.

<br>

## 제네릭(Generics) 정의
- 제네릭이란 타입을 마치 함수의 파라미터처럼 사용하는 것

<br>

## 기본적인 문법
- `<T>` 키워드 사용
```js
// JS 함수 사용 시
function logText(text){
    console.log(text);
    return text;
}

logText(10); // 숫자
logText('hello'); // 문자열
logText(true); // 진위값

// TS - 제네릭
function logText<T>(text: T):T{
    console.log(text);
    return text;
}

logText<string>('hello') // 함수를 호출할 때 파라미터에 대한 타입을 같이 지정하여 전달 
```

<br>

### 동작 과정을 세분화
1. 함수에 전달
```js
logText<string>('hello') // 인자를 넘기는데 인자의 타입은 string
```
<br>

2. 전달 받은 후
```js
// 구조
function logText<T>(text: T):T{
    console.log(text);
    return text;
}

// 아래와 같이 동작 되는 것처럼 보인다.
function logText<string>(text: string):string{
    console.log(text);
    return text;
}
```
<br>

![bandicam 2022-04-01 17-50-47-779](https://user-images.githubusercontent.com/87301268/161230597-0c9c0d85-bf4f-452e-9615-6c4bd732cd3b.jpg)

![bandicam 2022-04-01 17-57-44-882](https://user-images.githubusercontent.com/87301268/161230787-0f8755ef-68d5-4053-aed0-cc01cdf7dec7.jpg)

<br>


## 제네릭을 사용하면 좋은 경우
### 코드 중복 선언의 단점
```js
function logText(text: string){
    console.log(text);
    text.split('').reverse().join('');
    return text;
}

logText('a');
logText(10);
logText(true);
```
- 만약 받은 값들 안에다가 `split` 이라는 문자열 API 를 사용하고 싶다는 가정
- 결국 문자열만 받을 수 없는 함수가 되어버립니다.
- 'a' 를 제외한 나머지 10, true 는 받고 싶지만 받지 못 합니다.
- 이런 상황을 해결하기 위해서 아래와 같이 함수를 작성하면 됩니다.

```js
function logNumber(num: number){
    console.log(num);
    return num;
}

logNumber(10);
```
- 물론 문제 없이 사용할 수 있습니다.
- 하지만 유지보수 관점에서는 좋지 않습니다. 이유는 단순히 타입을 다르게 받기 위해서<br> 중복되는 코드들을 생산하면, 가독성 뿐만 아니라 의미없이 코드가 커지게 됩니다.

<br>

### 이런 경우, 유니온 타입을 활용하면 ?
```js
function logText(text: string | number){
    console.log(text);
    text. // string 과 number 둘 다 접근 가능, 현재 타입은 string 과 number
    return text;
}

const a = logText('a');
console.log(a) // 여전히 string 과 number 둘 다 가지고 있습니다.
a.split('') // 에러가 발생합니다.
logText(10);
```
- 들어가는 Input 의 상황은 해결이 될 수 있습니다.
- 하지만, 코드 안에서의 `text` 의 타입은 string 과 number 둘 다 가지게 됩니다.
- 그 후 logText 를 이용해서 문자열을 집어넣고 나서, 리턴 값을 받고 console.log 로 확인 하면
- 여전히 string 과 number 둘 다 가지고 있습니다.
- 문자를 넣었지만, `split` API 를 사용할 수 없습니다. 결론적으로 a 는 string이 아니라 <br>
`string | number` 타입 입니다.
- __반환 값에 문제점이 생기게 됩니다.__

![bandicam 2022-04-01 20-58-03-879](https://user-images.githubusercontent.com/87301268/161258985-d3a53377-b865-4a34-a052-681faf6a6f00.jpg)

<br>

### 제네릭을 사용하면 문제 없이 작동할 수 있다.
```js
function logText<T>(text: T): T{
    console.log(text);
    return text;
}

//const str = logText('a');
const str = logText<string>('a');
str.split(''); // 에러가 발생하지 않는다.

```
- 제네릭은 타입 정의에 대한 이점을 가져갑니다.
- 코드를 중복 선언 할 필요 없이 타입을 비워놓은 상태에서 타입에<br> __어떤 타입이 들어갈지는 호출하는 시점에서 정의하는 것__ 

- 또한 타입을 추론을 하여 최종 반환 값까지 붙일 수 있는 게 제네릭 입니다. 

<br>

## 인터페이스에 제네릭을 선언하는 방법
### 인터페이스 선언
```js
interface Dropdown{
    value: string;
    selected: boolean;
}

const obj: Dropdown = { value: `abc`, selected: false};
```

### 인터페이스 + 제네릭
```js
interface Dropdown<T>{
    value: T;
    selected: boolean;
}

const obj: Dropdown<string> = {value: 'abc', selected: false};

//const obj: Dropdown<Number> = {value: 'abc', selected: false}; // value 타입 에러 발생
```

<br>

## 제네릭의 타입 제한
```js
function logTextLenght<T>(text: T): T {
    console.log(text.length) 
    // 에러, 타입스크립트 입장에서는 logTextLenght에 어떤 타입이 들어올지 예측할 수 없기 때문에
    // 개발자만 아는 사실
    return text;
}

logTextLenght('hi');
```
![bandicam 2022-04-01 21-51-03-146](https://user-images.githubusercontent.com/87301268/161267108-5df12aa0-8959-42a0-a4fa-861bc1d29e1d.jpg)

<br>

- 아래 처럼 작성
```js
function logTextLenght<T>(text: T[]): T[] {
    // 함수 내부 안에서 배열임을 알 수 있다.
    console.log(text.length)
    text.forEach(function (text){

    }); 
    return text;
}

logTextLenght<string>(['hi', 'abc']);
```

<br>

## 제네릭 타입 제한 2 - 정의된 타입 이용하기
```js
interface LenghtType {
    lenght: number;
}

function logTextLenght<T extends LenghtType>(text: T): T{
    text.lenght;
    return text;
}

logTextLenght(10); // 오류, 숫자는 `lengh` 가 제공 되지 않습니다.
logText({ length: 0, value: 'hi' }); // `text.length` 코드는 객체의 속성 접근과 같이 동작하므로 오류 없음
```

<br>

## 제네릭의 타입 제한 3 - keyof
```js
interface ShoppingItem {
    name: string;
    price: number;
    stock: number;
}

// shoppingItem 에 있는 key 들 중에 한 가지가 제네릭이 된다.
// 즉, getShoppingItemOption 의 파라미터로는 'name', 'price', 'stock' 중 한 가지만 들어 갈 수 있다.
function getShoppingItemOption<T extends keyof ShoppingItem>(itemOption: T): T {
    return itemOption;
}

getShoppingItemOption("name")

// getShoppingItemOption(10); // 에러 발생
// getShoppingItemOption<string>('a'); // 에러 발생
```

![bandicam 2022-04-01 22-19-20-535](https://user-images.githubusercontent.com/87301268/161271557-909083ee-0f36-42c2-94a6-dc1bc50ed9f8.jpg)

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/generics.html#%EC%A0%9C%EB%84%A4%EB%A6%AD-%EC%A0%9C%EC%95%BD-%EC%A1%B0%EA%B1%B4)