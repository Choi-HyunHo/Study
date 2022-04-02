# 타입 단언(Type Assertion)
타입스크립트 보다 개발자가 더 타입을 잘 알고 있다는 가정을 하고<br>
'개발자가 정의한 타입으로 간주를 해라' 라는 의미 입니다.

<br>

## `as` 키워드 사용한 후 타입 적기

```js
let a; // 타입 추론에 의해 any 타입
a = 20;
a = 'a';

let b = a; 
// a의 타입이 바뀌고 할당을 하여도 아직 타입 추론에 의해 a: any 타입
// 맨 처음 할당한 타입의 값이 그대로 b에 들어가서 b: any 타입 
```

- 개발자가 보는 관점에서는 `a` 가 `string` 이 될 것이라고 알 수 있습니다.
- 하지만 타입스크립트 관점에서는 그렇지 않습니다.

- 아래와 같이 사용하면 타입이 개발자의 관점으로 단언이 됩니다.

<br>

```js
let b = a as string; 
```
- `a` 라는 것은 `string` 으로 간주 합니다.
- 즉, `b` 의 타입은 `string` 이 됩니다.

![bandicam 2022-04-02 18-15-36-510](https://user-images.githubusercontent.com/87301268/161376459-8c774715-f6a7-45f3-8dd2-bb74c036c43a.jpg)

<br>

## DOM API 사용 예제
### 만약 타입 검사가 엄격한 프로젝트를 하고 있는 경우
```js
let div = document.querySelector('div')
div.innerText
```
![bandicam 2022-04-02 18-26-23-091](https://user-images.githubusercontent.com/87301268/161376936-d74f5de9-b947-4621-93a3-1faff8ecf557.jpg)
![bandicam 2022-04-02 18-27-14-694](https://user-images.githubusercontent.com/87301268/161376948-fd8c209d-da1f-4036-89a6-89ac34c9a7f7.jpg)

<br>

### `null` 아니라는 것을 보장을 아래 코드 처럼 타입을 보장 해야 합니다.
```js
let div = document.querySelector('div')
if (div){
  div.innerText
}
```

<br>

### 이 때, 타입 단언을 할 수 있습니다.
```js
let div = document.querySelector('div') as HTMLDivElement;
div.innerText
```
![bandicam 2022-04-02 18-31-52-541](https://user-images.githubusercontent.com/87301268/161377085-93aeb02b-3401-46a6-aab9-33deb6f96749.jpg)

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)