# 문서 객체 모델(DOM, Document Objects Model)

__XML이나 HTML 문서에 접근하기 위한 일종의 인터페이스입니다.__ <br>

이 객체 모델은 문서 내의 모든 요소를 정의하고, 각각의 요소에 접근하는 방법을 제공합니다.

<br>

|  ||
|---|:---:|
| 넓은 의미 | 웹 브라우저가 HTML 페이지를 인식하는 방식 |
| 좁은 의미 | Document 객체와 관련된 객체의 집합 | 

<br>

- DOM 안에는 우리가 정의한 요소들이 Tree 형태로 구성되어 있습니다. 이것을 이용하여 JS로 웹페이지를 제어할 수 있습니다.

- HTML의 `<head>`, `<body>` 그리고 `<body>` 의 안에 각각의 tag 들을 `요소(element)`라고 부르고, JS에서는 `문서 객체`라고 부릅니다.

- DOM을 이용하면 HTML 태그를 추가, 수정,삭제 등 할 수 있습니다.

<br>

## DOM Tree
![KakaoTalk_20220118_182447356](https://user-images.githubusercontent.com/87301268/160579780-cd9aaafc-d2f8-4459-8c59-687900c39550.jpg)



<br>

## 정리

1. 브라우저가 HTML 파일을 읽을 때 브라우저가 이해할 수 있는, 메모리에 보관할 수 있는 object로 변환 합니다.

2. object로 변환 후, Tree 구조를 만듭니다. 이것이 DOM 입니다.

<br>

## 참고
- [MDN, Dom](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)
- [코드 안식처](https://blog.naver.com/x7788/222625070979)
- http://www.tcpschool.com/javascript/js_dom_concept
- https://blog.naver.com/hunii123/222624280585