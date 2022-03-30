# AJAX 
Asynchronous Javascript And Xml(비동기식 자바스크립트와 xml)의 약자

<br>

## AJAX 란
- 자바스크립트를 통해서 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있는 통신 기능 입니다.

- `Ajax` 를 사용하면 웹 애플리케이션은 기존 페이지의 화면 및 동작을 방해하지 않으면서<br> 백그라운드에서 비동기적으로 서버로 데이터를 보내고 서버에서 데이터를 받아올 수 있습니다

- `XMLHttpRequest` API는 비동기 통신에 자주 사용되며, 최근에는 `fetch` API가 자주 사용됩니다.

<br>

## 비동기 통신
<img width="709" alt="스크린샷 2021-08-26 오후 3 36 24" src="https://user-images.githubusercontent.com/87301268/160783846-d29d2e70-f47f-44a9-9549-7acb21c860b5.png">

- 요청을 보낸 후 응답과는 상관없이 동작하는 방식

- 웹페이지를 리로드 하지 않고도 통신이 가능한 구조

<br>

## AJAX 의 장점
1. __상호작용성이 좋아집니다.__ 서버의 새로운 컨텐츠를 전체 페이지를 다시로드할 필요 없이 동적으로 변경할 수 있습니다.
2. 스크립트나 스타일 시트는 한 번만 요청하면 되므로 __서버에 대한 부담을 줄여줍니다.__

3. 상태를 페이지에서 관리 할 수 ​​있습니다. 메인 컨테이너 페이지가 다시 로드되지 않기 때문에 JavaScript의 변수와 DOM의 상태가 유지됩니다.

<br>

## AJAX 의 단점
1. 동적 웹 페이지는 북마크하기 어렵습니다.
2. 브라우저에서 JavaScript가 비활성화된 경우 작동하지 않습니다.

3. 일부 웹 크롤러는 JavaScript를 실행하지 않으며 JavaScript에 의해 로드된 콘텐츠를 볼 수 없습니다.


<br>

## 참고
- [frontend-handbook](https://www.frontendinterviewhandbook.com/kr/javascript-questions/#ajax%EC%97%90-%EB%8C%80%ED%95%B4-%EA%B0%80%EB%8A%A5%ED%95%9C-%ED%95%9C-%EC%9E%90%EC%84%B8%ED%9E%88-%EC%84%A4%EB%AA%85%ED%95%98%EC%84%B8%EC%9A%94)
- [비동기 통신 이해하기](https://velog.io/@dasssseul/JS-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%86%B5%EC%8B%A0-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0XHR%EA%B3%BC-Fetch-API)
- [MDN, Ajax](https://developer.mozilla.org/ko/docs/Web/Guide/AJAX)