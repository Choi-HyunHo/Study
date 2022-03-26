# Callback Function

## 함수
- 프로그램 안에서 각각의 작은 기능들을 수행하는 것
- 재사용이 가능 합니다.
- 하나의 함수는 한 가지의 일만 하도록 만드는 것이 좋습니다.
- 함수의 이름을 보고 함수의 기능을 알 수 있어야 합니다.

```js
// 기본형
function 함수명 (){
     코드
};
함수명() // 호출

// 매개변수
function 함수명 (매개변수){
     코드
};
함수명(매개변수) // 호출
```

<br>

## 콜백함수 ?

- 함수 안에 함수를 전달해서 상황에 맞게 함수를 부르는 것
- 즉, __파라미터(매개변수)로 함수를 전달하여 함수의 내부에서 실행되는 함수를 콜백함수라 합니다.__
```js
// printYes, printNo 등 함수가 파라미터로 전달되어
// 조건에 맞게 함수가 호출된다.
function random(answer, printYes, printNo) {
  if (answer === 'apple') {
    printYes()
  } else {
    printNo()
  }
}

// 익명 함수
const printYes = function () {
  console.log('yes')
}

const printNo = function print() {
  console.log('no')
}
random('apple', printYes, printNo) // yes
random('banana', printYes, printNo) // no
random('melon', printYes, printNo) // no
```

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222622515672)
- https://ko.javascript.info/callbacks
- [자바스크리트_콜백함수](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%9C%EB%B0%B1-%ED%95%A8%EC%88%98)
- https://deftkang.tistory.com/20