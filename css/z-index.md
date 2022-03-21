# z-index

z-index 속성은 겹치는 요소의 쌓임 순서를 제어 합니다.<br>
z-index 는 position 의 `absolute`, `relative`, `fixed` 일 때만 동작하며 음수를 사용할 수 있습니다.<br>
즉, `position: static` 이 아닌 값을 갖는 요소에만 영향을 줍니다.

<br>

## z-index 규칙
- position 속성이 없는 태그들은 나오는 순서대로 쌓입니다.
- position 속성이 있는 태그들은 없는 태그들보다 위에 나오는 순서대로 쌓입니다.
- position 속성에 z-index까지 있다면 z-index가 큰 태그가 위에 쌓입니다.
- z-index이 같은 값일 경우 HTML 구조상 아래인 것이 먼저 적용 됩니다.

- z-index가 아무리 크더라도 부모 태그의 z-index가 더 우선입니다.


<br>

## z-index 종류
```css
z-index : auto | number 
```
- auto : 문서 흐름에 따른 기본 위치에 위치 (기본값)

- number : 숫자 클수록 겹쳤을 때 위에 위치하고, 음수가 될 경우 아래에 위치
<br>

## z-index 예시
```html
<div class="container">
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
    <div class="box4"></div>
</div>
```
```css
.container{
    width: 500px;
    height: 500px;
    position: relative;
    background-color: silver;
}

div {
    width: 100px;
    height: 100px;
}

.box1 {
    background-color: red;
    position: absolute;
    top: 50px;
    left: 80px;
    z-index: 3;
}

.box2 {
    background-color: orange;
    position: absolute;
    top: 70px;
    left: 100px;
    z-index: 3;
}

.box3 {
    background-color: blue;
    position: absolute;
    top: 90px;
    left: 120px;
    z-index: 2;
}

.box4{
    background-color: hotpink;
    position: absolute;
    top: 120px;
    left: 150px;
    z-index: 4;
}
```
![캡처](https://user-images.githubusercontent.com/87301268/159275824-83dca41d-e059-46c7-888a-fb988f74a5d4.JPG)



<br>

## 참고
- https://homzzang.com/b/css-113
- https://be-a-weapon.tistory.com/112
- https://www.zerocho.com/category/CSS/post/5a18b330e9c0ec001b08238e
- https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context
- https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index/Adding_z-index