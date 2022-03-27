# `<script>` 태그는 어디에 위치해야 할까?

일반적으로 `<body>` 태그 가장 최하위에 많이 사용 합니다.<br>
하지만 위의 방법 말고도 여러 가지 방법이 있습니다.

<br>

## 브라우저 동작 방식 ( 렌더링 순서 )


![Web  Rendering (DOM_CSSOM_Render Tree)](https://user-images.githubusercontent.com/87301268/160271212-3e26361a-57e7-4984-93a5-41c467d76adb.png)

- `requests / response` :  브라우저가 서버에게 HTML을 요청
- `loading` : 서버에게서 HTML을 뱓아 파싱
- `scripting`  : html 파일은 DOM 요소, CSS 파일은 CSSOM 요소로 변환
- `rendering` : 최종적으로 브라우저에 보여질 요소들만 Render Tree 생성 
- `layout`  : 각 요소들이 가지고 있는 스타일에 따라 브라우저 화면에서의 위치와 크기 계산

- `printing` : 계산이 완료되면 요소들을 브라우저에 표시

<br>

## HTML을 파싱하다가 `<script>` 태그를 만나게 되면 ?
- 브라우저는 HTML 태그들을 읽어나가는 도중 `<script>` 태그를 만나면 파싱을 중단하고
- javascript 파일을 로드 후 javascript 코드를 파싱합니다. 

- 완료되면 그 후에 HTML 파싱이 계속 됩니다.

<br>

## 만약 `<head>` 태그 안에 위치하거나, 일반 태그들 사이에 위치하게 된다면 ?
-  HTML을 읽는 과정에 스크립트를 만나면 중단 시점이 생기고 그만큼 Display에 표시되는 것이 지연 됩니다.

<br>

## `<body>` 최하단에 사용하는 이유

- 브라우저가 HTML을 위에서 아래로 쭉 분석하고, 페이지가 준비가 된 다음, 마지막에 JS 파일을 서버에서 받아오고 실행합니다.<br>
사용자들에게 JS 파일을 받기 전에 이미 준비가 되어서 무리없이 접근이 가능합니다.

<br>

### 하지만 최하단에 사용해도 문제는 있습니다.
- 만약 웹 사이트가 Javascript에 의존도가 크면, 즉 사용자가 의미있는 contents 를 보기 위해서 Javascript를 통해 서버에서 데이터를 <br>불러온다던지,
 이벤트 효과를 많이 사용한 경우라면 정상적인 페이지를 보기 전에는 서버에서 JS파일을 다운받는 시간을 기다려야하고,<br> 실행하는 시간도 기다려야 합니다.

<br>

## `<head>` 안에 `<script async src=” ”>`
```html
<script async src="">
```
![image](https://user-images.githubusercontent.com/87301268/160273254-e3e051a7-646a-48dc-998b-e4265c8b2adb.png)

1. 브라우저가 HTML을 분석 하다가, `async` 를 발견하면 병렬로, ‘JS파일을 다운로드 받자’고 명령을 합니다. ( 실행은 아직! )
2. 다시 HTML을 분석하고 JS파일이 다운로드가 완료되면, 그 때 HTML을 분석하는 것을 멈추고, 다운로드 된 JS파일을 실행 합니다. 

3. 실행을 다 하고 나서 나머지 HTML을 분석 합니다.

<br>

### 장점
- `<body>` 끝에 사용하는 것 보다는 JS파일 다운이 HTML 파싱 하는 동안 같이 일어나서 다운로드 받는 시간을 절약할 수 있습니다.


<br>

### 단점
- Javascript가 HTML이 완전히 분석되기 전에 실행이 되기 때문에, 만약 쿼리 셀렉터를 통한 DOM 요소를 조작 한다하면,<br>
 조작 할려고 하는 시점에 우리가 원하는 HTML 요소가 정의되어 있지 않을 수 있기 때문에 위험할 수도 있습니다

- HTML을 분석하다가 Javascript를 실행하기 위해 멈출 수 있기 때문에, 사용자가 페이지를 보는데 시간이 여전히 걸릴 수 있습니다. 

<br>

## `<head>` 안에 `<script defer src=” ”>`
```html
<script defer src="">
```
![image (1)](https://user-images.githubusercontent.com/87301268/160273379-717190a4-5b36-47c9-ac9d-5c012fb93be7.png)

1. 브라우저가 HTML을 분석 하다가, defer를 발견하면 ‘JS파일을 다운로드 받자’ 하고 명령만 합니다.
2. 그 후 나머지 HTML을 끝까지 분석 합니다. 

3. 그리고 마지막에 다운로드 받아진 JS파일을 실행 합니다.

- 이런 방식으로 하게 되면 사용자에게 완전히 파싱 된 HTML을 보여주게 되어 기본적인 방식에서 나타나는 문제점을 해결하게 됩니다.

<br>

### 장점
- 각 방식에서 나오는 문제점들을 해결 가능 합니다.
- 가장 좋은 방법 입니다.

<br>

## 참고
- https://mongtak.tistory.com/28
- https://codingnotes.tistory.com/44
- https://brunch.co.kr/@jiwonleeqa/136
- [어디에 위치해야 할까](https://velog.io/@takeknowledge/script-%ED%83%9C%EA%B7%B8%EB%8A%94-%EC%96%B4%EB%94%94%EC%97%90-%EC%9C%84%EC%B9%98%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C%EC%9A%94)
- [코드 안식처](https://blog.naver.com/x7788/222489221940)
- [드림코딩](https://www.youtube.com/watch?v=tJieVCgGzhs&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=2)