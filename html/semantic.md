# 시맨틱 마크업(Semantic Markup)

'Semantic' 이란 의미론적인 뜻을 가지며 'Markup' 은 HTML문서 태그로 문서를 작성하는 것을 말합니다.
<br>

따라서, __시멘틱 마크업이란 의미가 잘 전달되도록 HTML문서를 작성하는 것을 말합니다.__

<br>

## 사용하는 이유
1. 검색 엔진 최적화(SEO) <br>
: 검색엔진이 시맨틱 태그를 중요한 키워드로 간주하기 때문에 검색엔진 최적화(SEO)에 유리합니다.
<br>

2. 웹 접근성 <br>
: 시각장애인들도 웹을 편리하게 접할 수있도록 도와줍니다.
<br>

3. 유지보수 <br>
: 정리된 마크업은 코드 식별에 용이합니다.

<br>


## 작성방법

시맨틱 마크업을 하기 위해선 각 태그를 그 용도에 맞게 사용해야 합니다.

<br>

- 헤더/푸터에 `<header>` 와 `<footer>` 사용
- 메인 컨텐츠에 `<main>` 과 `<section>` 사용
- 독립적인 컨텐츠에 `<article>` 사용
- 최상위 제목으로 `<h1>` 사용
- 순서가 없는 목록으로 `<ul>` 과 `<li>` 사용
- 내비게이션에 `<nav>` 사용

<br>

이런 식으로 태그가 가지고 있는 의미에 맞게 사용하는 것인데, 이런 점 이외에도 CSS 스타일을 명시하는 태그를 사용하지 않는 것 또한 시맨틱 마크업의 한 종류 입니다.

<br>

-  즉, 태그가 가지는 의미 자체가 스타일이라면 이는 마크업 자체가 스타일을 갖는 것이기 때문에 시맨틱 마크업에 적합하지 않습니다.

<br>

예를 들어, 동일한 효과를 부여하는 `<strong>` 과 `<b>` 태그가 있습니다. 둘은 동일하게 글자색을 진하게 하지만 `<b>` 태그의 경우는 그 자체가 "bold" 의 약어이기 때문에 태그 자체가 스타일을 가진다고 할 수 있습니다. 하지만 `<strong>` 의 경우는 "그 안의 내용이 다른 내용보다 더 강조되어야 한다" 라는 의미를 가지기 때문에 시맨틱 마크업에 더 적합 합니다.

<br>
<br>

## 예시 
<br>

![시맨틱마크업!](https://user-images.githubusercontent.com/87301268/157240759-6c79e8eb-776b-463b-9d7f-f3668b58e68b.jpg)


<br>

## 참고
- [MDN, Semantics](https://developer.mozilla.org/ko/docs/Glossary/Semantics)
- [www.w3schools.com/html/html5_semantic_elements.asp](www.w3schools.com/html/html5_semantic_elements.asp)
- [https://www.thinkful.com/](https://www.thinkful.com/)
- [https://velog.io/@hyun_sang](https://velog.io/@hyun_sang/HTML-%EC%8B%9C%EB%A7%A8%ED%8B%B1-%EB%A7%88%ED%81%AC%EC%97%85)