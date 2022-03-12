# id & class
html의 특정 요소에만 다른 효과를 주고 싶을 때, 해당 요소에 id 값 또는 class 명을 할당하여 처리가 가능 합니다.

<br>

## id
- id를 불러올 때 __샵(#)__ 를 사용 합니다.
- 하나의 요소에 하나의 id만 사용 가능 합니다.<br>
    -> 중복으로 사용 불가능

- 하나의 아이디는 페이지에서 딱 한 번만 사용해야 합니다.

<br>

## id 예제
```html
<h1 id="title">제목</h1>  
<h1 id="title">제목2</h1> <!--사용 불가-->  
<h1 id="title title2 title3">제목</h1> <!--사용 불가-->  
```
```css
#title{
    font-size : 20px;
}
```
<br>

## class
- class를 불러올 때 __마침표(.)__ 를 사용 합니다.
- 하나의 요소에 여러 개의 class 를 사용 할 수 있습니다.<br>
    -> class를 추가할 때는 띄어쓰기로 구분


- 중복 사용이 가능하며, 동일한 클래스명을 페이지의 여러 곳에 사용 할 수 있습니다.

<br>

## class 예제
```html
<h1 class="title">제목</h1>  
<h2 class="title">제목</h2> 
<h3 class="title color size">제목</h3>   
```
```css
.title{
    font-size : 20px;
}

.color{
    background-color : red;
}
```
<br>

## 정리
- id와 class의 차이는 id는 유일한 요소에 적용할 때, 그리고 css는 복수의 요소에 적용할 때 사용 

- 하나의 id는 한 문서에서 한 번만 사용이 가능하지만, 하나의 class는 여러 번 사용이 가능. 우선순위는 id가 class보다 높습니다.



<br>

## 참고
- https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Selectors
- https://blog.naver.com/x7788/222449322956
- https://heinafantasy.com/155