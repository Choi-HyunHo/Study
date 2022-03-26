# 변수(variable)

 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙혀진 이름

 <br>

 ## 식별자(Identifier)
 - 메모리 공간에 저장된 값을 식별할 수 있는 고유한 이름
 - 어떤 값을 구별해서 식별할 수 있는 고유한 이름
 
 - `변수의 이름` 을 식별자라고 표현하기도 합니다.

 <br>

## 변수명 네이밍 규칙

1. 변수명에는 오직 문자와 숫자, 그리고 기호 $와 _만 들어갈 수 있습니다.

2. 첫 글자는 숫자가 될 수 없습니다.
3. 예약어는 식별자로 사용할 수 없습니다.
4. ES6부터는 유니코드를 지원 합니다.
5. 대문자와 소문자를 구별 합니다.

<br>

## 카멜 표기법(camelCase)
- 여러 단어를 조합하여 변수명을 만들 때 주로 사용 합니다.

- 첫 단어를 제외한 각 단어의 첫 글자를 대문자로 작성합니다.
```js
let myVeryLongName
```

<br>

## 바람직한 변수명
- 변수명은 간결하고, 명확해야 합니다.
- `userName` 이나 `shoppingCart`처럼 사람이 읽을 수 있는 이름을 사용

- 무엇을 하고 있는지 명확히 알고 있지 않을 경우 외에는 줄임말이나 a, b, c와 같은 짧은 이름은 피하는게 좋습니다.

<br>

## 참고
- https://ko.javascript.info/variables
- https://velog.io/@jjg2362/Javascript%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EB%B3%80%EC%88%98