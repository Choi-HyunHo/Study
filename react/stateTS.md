# React + TypeScript(state)

기존의 useState 사용 방식

```js
const [count, setCount] = useState(초기 값);
```

- useState 를 사용하면 value 를 얻게 됩니다.
- 첫 번째 값은 state 의 value 입니다.
- 두 번째 값은 value 의 상태 변화 함수 입니다.

***
<br>

## useState 에 초기 값을 넣으면
```js
const [count, setCount] = useState(1);
```

![](https://velog.velcdn.com/images/hoho_0815/post/b7889d1c-a1b6-4e4d-8529-e15ef3ef695a/image.png)

- count 에 숫자 값이 들어온다고 TypeScript 가 초기 값을 가지고 추론을 한 것 입니다.

또한
```js
const [count, setCount] = useState(1);

setCount(2);
```
- 아무 일도 생기지 않습니다. 작성해야 하는 대로 작성을 했기 때문에

![](https://velog.velcdn.com/images/hoho_0815/post/d914c8e6-5c64-41b7-b299-c136dd3ef91d/image.png)

- 하지만 만약 string 타입을 보내게 되면, 에러가 있다고 말해줍니다.
- 왜냐하면 TypeScript 가 보기에 개발자가 state 를 만들고 초기 값을 1을 주어 숫자로 시작하고 있다면
- 계속 숫자 데이터를 사용할 것이라고 생각 합니다. ( 이것은 초기값을 어떤 형태로 주냐에 따라 동일 합니다. )

***
<br>

## 추론말고 사용하기
```js
const [value, setValue] = useState(0);
```

<br>

1. value 는 string 또는 number 타입이 될 수 있다고 알리기
```js
const [value, setValue] = useState<number | string>(0);
```

![](https://velog.velcdn.com/images/hoho_0815/post/7a3c034f-5c07-4a24-92d1-511801ddda97/image.png)

- 에러가 발생하지 않습니다.
- 하지만 setValue 의 값을 true 로 설정하면 에러가 발생 합니다.
- 왜냐하면, 타입 선언을 하지 않았기 때문입니다.

<br>

2. 하지만 state 를 만들면 보통 같은 타입으로 지속적으로 갑니다.
- 만약 false 로 시작하면 true 와 false 사이에서 바뀌게 될 것 입니다.
- 하지만 TypeScript 는 선언을 하지 않아도 초기 값에 들어오는 것에 따라서 충분히 추론을 할 수 있습니다.
```js
const [value, setValue] = useState(잘 넣어주기!)
```

***
<br>

## 참고
- 노마드코더