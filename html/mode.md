# 표준모드(Standards mode) & 호환 모드(Quirks mode)

과거의 웹 페이지는 넷스케이프와 익스플로러 버전이 따로 존재했고 웹 표준이 없었습니다. 그러나 W3C가 웹 표준을 만들면서 기존 브라우저가 새롭게 만들어진 표준을 기반으로 웹사이트를 제대로 표현할 수 없게 되자 렌더링을 할 때 표준 모드(Standards mode)와 호환 모드(Quirks mode)로 렌더링을 할 수 있게 옵션을 제공 하였습니다.
<br>

__이때, 브라우저가 DOCTYPE 을 보고 가지고 있지 않으면 호환 모드로 렌더링을 하고, 가지고 있다면 주어진 DOCTYPE 에 맞게 표준 모드로 렌더링을 합니다.__

<br>

### 차이점

1. 호환 모드
<br> : 오래된 웹페이지들을 최신 버전의 브라우저에서도 깨지지 않게 하기 때문에 각 브라우저마다 다르게 보일 수 있습니다.
<br> 즉, 각 브라우저 별 문서를 나타내는 방식이 다르기 때문에 크로스 브라우징 이슈가 발생 할 수 있습니다.
<br>

2. 표준 모드
<br> : DOCTYPE 를 선언하게 되면 주어진 DOCTYPE 에 맞게 렌더링을 합니다.

<br>

### 참고
- [MDN, 호환 모드와 표준 모드](https://developer.mozilla.org/ko/docs/Web/HTML/Quirks_Mode_and_Standards_Mode)
- [쿼크모드(Quirks mode)와 표준모드(Standard mode)](http://chongmoa.com/441)