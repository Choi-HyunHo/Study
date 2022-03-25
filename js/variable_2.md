# 변수 선언, 초기화, 할당
컴퓨터에 변수의 정체를 알리고, 공간을 확보하고, 원하는 값을 주는 과정이 변수 선언, 초기화, 할당 과정 입니다.

<br>

## 선언

- 선언 단계에서는 `const`, `let`, `var` 중 필요한 선언 키워드를 사용 합니다.

- 변수명을 실행 컨텍스트의 변수 객체에 등록하여 자바스크립트 엔진에 변수의 존재를 알립니다.



<br>

## 초기화
- 선언 키워드를 통해 이름이 정해진 변수에 값을 저장하기 위한 메모리 공간을 확보하고, 암묵적으로 `undefined` 를 할당 합니다.

- 또한 선언 키워드에 따라서 과정이 달라집니다. 

<img src="https://user-images.githubusercontent.com/87301268/160074297-3c7e90a1-6011-453a-8226-3b2226c0359b.jpg" width=700px height=300px background = white>


<br>


### var

- 선언과 초기화가 함께 일어나기 때문에, 메모리를 갖고 있는 변수로 간주되어 선언문 앞에서도 해당 변수는 참조가 가능하게 됩니다.

- `var` 로 선언한 변수는 실행 컨텍스트 최상위로 이동하게 되고,
var 키워드로 선언한 변수가 호이스팅이 일어나는 과정이라고 볼 수 있습니다.

<br>

### let
- 실행 컨텍스트에 변수를 등록하는 선언 단계를 거치고 난 후
해당 과정에서는 아무런 작업이 일어나지 않습니다.
- 선언과 초기화 단계가 나누어집니다.
- 초기 값을 지정하지 않는 다면, 변수는 값이 할당될 때까지 `undefined` 값을 가지게 됩니다.

- 초기화 단계는 런 타임 과정에서 발생 합니다.
```js
let byeonSu = 10; // 실제 코드

// 실제 코드를 런타임 과정 시점으로 살펴본다면,
let byeonSu // 초기화 (1) = undefined
byeonSu = 10; // 할당 (2) = 해당 메모리 주소값을 변수에 저장
```

<br>

### const
- `var` 와 `let` 과 달리 변수에 초기 값을 지정하지 않은 채 선언하면 구문에러(SyntaxError)가 뜹니다.

- `const` 로 변수를 선언할 때는 반드시 초기 값을 지정해줘야 합니다.
```js
const apple;  // SyntaxError
const a, b, c;  // SyntaxError
const i = 0; j = 1; k = 2;
```

<br>

## 할당
- `=` 를 통해 이루어지며 모든 선언 키워드의 할당은 런 타임 과정에서 이루어집니다.

<br>

## 정리
- `var` 는 실행 컨텍스트 Execution phase(실행 단계)에서 선언과 초기화 단계가 이루어지고, 런 타임 과정에서 할당이 이루어집니다.

- `let`, `const` 은 실행 컨텍스트 Execution phase(실행 단계)에서 선언 단계만 이루어지고, <br>런 타임 과정에서 초기화 단계 후 새로운 메모리를 확보하여 할당할 값을 저장 합니다.
<br> 그리고 초기화 단계에서 undefined가 저장 되어있는 주소 값을 할당 값이 저장된 주소 값으로 교체하여 할당 단계가 이루어집니다.


<br>

## 참고
- https://velog.io/@tess/%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8-%EC%B4%88%EA%B8%B0%ED%99%94-%ED%95%A0%EB%8B%B9
- https://eunjinii.tistory.com/119
- https://bokki.org/javascript/%ED%83%80%EC%9E%85-%EA%B0%92-%EB%B3%80%EC%88%98/javascript-%EB%B3%80%EC%88%98var-let-const/
- https://velog.io/@gkdlvj1214/%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8-%EC%B4%88%EA%B8%B0%ED%99%94-%ED%95%A0%EB%8B%B9-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85