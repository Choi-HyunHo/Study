# em & rem

상대적인 반응형 유닛
- 기본적으로 브라우저에서 HTML에 할당 되는 폰트 사이즈는 `16px` 입니다.

<br>

## 사용하는 이유 ?

- 폰트 사이즈를 `px` 만을 사용하여 나타내면 컨테이너의 사이즈가 변경되어도 컨텐츠가 그대로 고정된 값으로 유지됩니다.

- 또한, 사용자가 브라우저에서 폰트 사이즈 설정을 바꿔도 전혀 반응하지 않습니다.

<br>

## em

- 타이포그래피에서 현재 지정된 포인트 사이즈를 나타내는 단위 입니다.

- __`em` 의 경우 해당 단위가 사용되고 있는 부모요소의 font-size 속성 값이 기준 입니다.__

```html
<div class = "parent">
    Parent
    <div class="child">Child</div>
</div>
```
```css
.parent{
    font-size : 8em; 
}

.child{
    font-size : 0.5em;
}
```

- 가장 위에서 "기본적으로 브라우저에서 HTML에 할당 되는 폰트 사이즈는 `16px` 입니다." 라고 설명 했습니다.

- 그 말은 즉, 사용자가 따로 HTML이나 body에 폰트 사이즈를 지정하지 않는 이상 기본적으로 `16px` 로 지정이 됩니다. 
- parent에 8em 이라는 것은, parent의 부모 요소인 HTML 의 `16px * 8 ` 이 계산된 값 입니다. (parent는 128px)
- parent 안에 있는 child는 부모의(parent = 128px) 0.5배가 계산되어진 `64px` 입니다.

<br>

## rem

- `rem` 의 r은 root를 나타내는 입니다.

- __최상위 요소의 font-size 속성 값이 기준, 최상위 요소는 html 태그 입니다.__

```html
<div class = "parent">
    Parent
    <div class="child">Child</div>
</div>
```
```css
.parent{
    font-size : 8rem; 
}

.child{
    font-size : 0.5rem;
}
```
- parent는 루트 요소인, HTML에서 기본적으로 지정된 16픽셀의 8배가 곱해진 `128px`이  계산 됩니다.

- child는 루트 요소인, HTML에서 기본적으로 지정된 16픽셀에 0.5배가 곱해진 `8px` 이 계산 됩니다.

<br>

## 정리
- `em` 은 부모 요소의 상대적으로 크기가 결정이 됩니다.
- `rem` 은 루트 요소에 상대적으로 크기가 결정이 됩니다.

## 참고
- https://www.daleseo.com/css-em-rem/
- https://medium.com/watcha/watcha-%EA%B0%9C%EB%B0%9C-%EC%A7%80%EC%8B%9D-px-em-rem-f569c6e76e66
- https://www.youtube.com/watch?v=7Z3t1OWOpHo&list=PLv2d7VI9OotQ1F92Jp9Ce7ovHEsuRQB3Y&index=21