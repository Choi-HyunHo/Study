# position 속성

static, relative, absolute, fixed, sticky 가 있습니다.

<br>

## static

- 기본 상태이며, 차례대로 `왼쪽 → 오른쪽`, `위 → 아래` 위치 합니다.

```html
<section>
  <div></div>
  <div class = "box"></div>
  <div></div>
  <div></div>
  <div></div>
</section>
```
```css
body{
    margin: 0;
}

div{
  width : 50px;
  height : 50px;
  margin-bottom : 20px;
  background : red;
}

section{
  background : yellow;
}

.box{
  background-color : blue;
}
```
![bandicam 2022-03-15 18-15-28-091](https://user-images.githubusercontent.com/87301268/158345288-d26860bb-6f90-49a9-a4ce-9666839589ed.jpg)


- position을 지정하지 않으면 위와 같이 위에서 아래로(div는 block요소) 나타납니다.

<br>

## relative

- 기존 static이었을 때 위치를 기준으로 `top`, `right`, `bottom`, `left` 속성을 사용해 위치 조절이 가능 합니다.

- 아래 그림에서 `section` 태그에 `position : relative` 속성을 주고 변화를 보겠습니다.

```css
section{
  background : yellow;
  position: relative;
  left: 50px;
  top: 50px;
}
```
![bandicam 2022-03-15 18-20-12-739](https://user-images.githubusercontent.com/87301268/158346147-a734a003-c30a-418a-8ff4-cbb2a748e140.jpg)

- 위와 같이 원래 본인 위치에서 `top`, `left` 값 만큼 이동 하였습니다.

<br>

## absolute

- position이 static속성을 가지고 있지 않은 부모를 기준으로 이동 합니다. 만약 부모 중에 포지션이 relative, absolute, fixed인 태그가 없다면 가장 위의 태그(body)가 기준이 됩니다.

- HTML 코드는 첫 번째 예시와 동일하고 CSS 코드만 비교 하겠습니다.

```css
body{
    margin: 0;
}

div{
  width : 50px;
  height : 50px;
  margin-bottom : 20px;
  background : red;
}

section{
  background : yellow;
  position: relative;
  left: 50px;
  top: 50px;
}

.box{
  background-color : blue;
  position: absolute;
  left: 50px;
  top: 50px;
}
```

![bandicam 2022-03-15 18-23-15-297](https://user-images.githubusercontent.com/87301268/158347263-94daa6e9-6800-4eb5-a11e-5d43f64713fb.jpg)

![bandicam 2022-03-15 18-23-30-825](https://user-images.githubusercontent.com/87301268/158347314-de7c37c8-bda8-483b-ae9c-77b7fde690e0.jpg)

- 파란색 박스가 자신의 위치에서 벗어나 노란색 박스 영역을 기준으로 이동 하였습니다.

<br>

## fixed

- 태그의 본래 위치에서 제거되고 `뷰 포트`를 기준으로 지정된 특정 위치에 고정 됩니다.

```css
section{
  background : yellow;
  position: relative;
  left: 50px;
  top: 50px;
}

.box{
  background-color : blue;
  position: fixed;
  top: 20px;
  left: 10px;
}
```


![bandicam 2022-03-15 18-33-54-132](https://user-images.githubusercontent.com/87301268/158348654-edfa0ba8-2d2c-4fae-b4be-e229707fafb4.jpg)

<br>

## sticky

- 원래 본인의 자리에 위치를 하고, 스크롤링 하여 지정된 임계값을 넘으면 fixed로 처리가 됩니다.

- `top`, `left` 와 같은 위치값을 반드시 작성해줘야 sticky 속성을 적용할 수 있습니다.

```css
.box{
  background-color : blue;
  position: sticky;
  top: 10px;
  left: 70px;
}
```

![fixed](https://user-images.githubusercontent.com/87301268/158352886-371ed641-687e-4bd6-8f47-c5e111edaf8d.gif)

<br>

## 참고
- [MDN, position](https://developer.mozilla.org/ko/docs/Web/CSS/position)
- https://tech.lezhin.com/2019/03/20/css-sticky
- https://abcdqbbq.tistory.com/70
- https://www.youtube.com/watch?v=jWh3IbgMUPI&list=PLv2d7VI9OotQ1F92Jp9Ce7ovHEsuRQB3Y&index=8
