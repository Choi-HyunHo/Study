# 함수 선언문과 함수 표현식의 차이점

- 첫 번째 차이는 `문법적 차이` 입니다.
    - 함수 선언문: function 함수이름() { ... }

    - 함수 표현식: const func = function () { ... }


- 두 번째 차이는 자바스크립트 엔진이 `언제 함수를 호출하는지` 입니다. -> 호이스팅의 영향을 받는지
    - 함수 표현식은 `실제 실행 흐름` 이 해당 함수에 도달했을 때 함수를 생성 합니다. <br>따라서 __실행 흐름이 함수에 도달했을 때 부터 해당 함수를 호출__ 할 수 있습니다.
    
    - 함수 선언문은 __실제 실행 흐름이 함수 선언문이 정의되어 있는 부분에 도달하기 전에도 함수를 호출__ 할 수 있습니다.<br> 이는 `호이스팅` 에 영향을 받습니다.

<br>

## 예시
```js
// 실행 전
logMessage();
sumNumbers();

function logMessage() {
  return 'worked';
}

var sumNumbers = function () {
  return 10 + 20;
};

// 실행 시
function logMessage() {
  return 'worked';
}

var sumNumbers;

logMessage(); // 'worked'
sumNumbers(); // Uncaught TypeError: sumNumbers is not a function

sumNumbers = function () {
  return 10 + 20;
};
```

<br>


## 정리
- 문법적 차이
- 함수 선언식은 호이스팅에 영향을 받지만, 함수 표현식은 호이스팅에 영향을 받지 않습니다.

<br>

## 참고
- [캡틴판교](https://joshua1988.github.io/web-development/javascript/function-expressions-vs-declarations/)
- https://dc2348.tistory.com/18