# 커스텀 데이터 속성(date-*)

HTML에서 기본적으로 제공되는 속성이 아닌, 개발자가 임의의 속성을 추가하고자 할 때 사용 합니다.

<br>

## 특징
- 커스텀 데이터 속성은 html tag 상에서 별다른 작용을 하지 않습니다.
- 속성의 시작은 반드시 data-로 시작 
- 자바스크립트와 CSS에서도 커스텀 데이터 속성의 정보를 사용 할 수 있습니다.

- 브라우저는 커스텀 데이터 속성을 만나면 해석하지 않고 건너 뜁니다. 따라서 보여지는 화면에 아무런 영향을 주지 않습니다.

<br>

## HTML 사용

```html
<article
  id="electriccars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars">
</article>
```
- `data-이름` 으로 사용 할 수 있습니다.

-  어떤 엘리먼트든 data-로 시작하는 속성은 무엇이든 사용할 수 있습니다. 화면에 안 보이게 글이나 추가 정보를 엘리멘트에 담아 놓을 수 있습니다.

<br>

## CSS 사용

```CSS
article[data-columns='3'] {
  width: 400px;
}
article[data-columns='4'] {
  width: 600px;
}
```
- CSS의 속성 선택자도 데이터에 따라 스타일을 바꾸는데 사용할 수 있습니다.

<br>

## JavaScript 사용

```javascript

<article
  id="electriccars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars">
</article>

<script>

var article = document.getElementById("electriccars");

article.dataset.columns; // "3"
article.dataset.indexNumber; // "12314"
article.dataset.parent; // "cars"

</script>
```

- 자바스크립트에서 dataset 객체를 통해 data 속성을 가져올 수 있습니다.
- `data-` 뒷 부분을 사용 합니다.
- `엘리먼트.setAttribute("속성명", "속성값");` 을 사용하여 추가 가능합니다.

- 해당 속성은 문자열이며 읽거나 쓸 수 있습니다.

```javascript
article.dataset.columns = 5
```
- 해당 속성의 값을 변경도 가능 합니다.

<br>

## 주의점
1. 민감한 데이터는 넣지 않는 것이 좋습니다.<br> 
-> HTML에 데이터를 넣는 것은 누구에게나 보이고, 자바스크립트로 접근 가능하기 때문에 누구나 수정할 수 있습니다.

2. 관찰 해야하는, 접근 가능해야하는 중요한 내용은 데이터 속성에 저장하지 않는 것이 좋습니다.<br>
-> 접근 보조 기술이 접근할 수 없기 때문입니다. 또한 검색 크롤러가 데이터 속성의 값을 찾지 못하는 문제도 가지고 있습니다.

3. 브라우저별 호환성 문제



<br>

## 참고
- https://developer.mozilla.org/ko/docs/Learn/HTML/Howto/Use_data_attributes
- [https://velog.io/@yunsungyang-omc/](https://velog.io/@yunsungyang-omc/HTML-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%86%8D%EC%84%B1-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-data-attribute)