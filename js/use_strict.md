# 엄격모드(use strict)

ECMAScript5 부터 도입된 기능으로 __기존에 무시되던 에러들로 하여금 에러를 발생__ 시킵니다.

<br>

## 사용하는 이유

- 자바 스크립트는 기존 기능을 변경하지 않으면서 새로운 기능을 추가해왔기 때문에 호환성 문제가 없었습니다.

- 하지만 ES5부터 새로운 기능이 추가되고 기존 기능의 일부가 변경 되었습니다.
- 그에 따라 기존 기능이 변경되어 호환성에 문제가 생기게 되었고, 이를 방지하기 위한 것이 엄격 모드(strict mode) 입니다.

<br>

## 사용하는 방법
```js
"use strict";

// 이 코드는 모던한 방식으로 실행됩니다.
...
```
- 최상단에 선언 합니다.
- 위에 처럼 사용하면 파일 전체에 적용이되고, 원하는 함수 스코프에만 적용이 가능 합니다.

<br>

## 참고
- [MDN, use strict](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode)
- https://ko.javascript.info/strict-mode
- https://blogpack.tistory.com/972
- https://haenny.tistory.com/286