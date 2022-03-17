# Flex

flex는 1차원으로 수평, 수직 영역 중 하나의 방향으로만 레이아웃을 나눌 수 있습니다.

<br>

## display : flex

- 정렬 하고자 하는 요소를 감싸는 부모에게 `display : flex` 를 선언 합니다.
- flex 아이템들은 가로 방향으로 배치되고, 자신이 가진 내용물의 `width` 만큼만 차지 
합니다.

- 선언 시, 보이지 않는 두 개의 축(Axis)이 생깁니다.
```html
<div class="flexBox">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</div>
```
```css
.flexBox{
    display: flex;
}
```

<br>

## 메인축(Main Axis), 수직축 또는 교차축(Cross Axis)
![4-flexbox-axes](https://user-images.githubusercontent.com/87301268/158803449-193a75e2-a454-4184-beac-41c02122aee9.jpg)

- Main axis : flex-direction 방향에 따라

- Cross axis : main과 수직을 이루는 방향

<br>

## flex-direction

- 자식들이 배치되는 축의 방향을 결정하는 속성

```css
 .flexBox{
        display: flex;
        flex-direction: row;
	    /* flex-direction: column; */
	    /* flex-direction: row-reverse; */
	    /* flex-direction: column-reverse; */
}

```

- row가 기본 속성 값 입니다.
- `flex-direction : row` (왼->오) --> 왼쪽에서 오른쪽으로 가는 main axis 가 만들어집니다. 그리고, 그와 정확하게 수직을 이루는 cross axis가 나옵니다.(위->아래)
- `flex-direction : column` (위->아래) --> 위에서부터 아래로 흐르는 main axis 가 만들어집니다.
그리고, 그와 정확하게 수직을 이루는 cross axis 가 나옵니다.(왼->오)

- 그외 `row-reverse` 와 `column-reverse` 도 마찬가지 입니다. (reverse 는 반대로 뒤집어서)

<br>


<img  width="400" height="400" src="https://user-images.githubusercontent.com/87301268/158806376-f1d91f46-5531-4d38-a8a4-cd24b649c741.jpg"></img>
<img  width="400" height="400" src="https://user-images.githubusercontent.com/87301268/158807051-f39d50b8-8c02-434b-9df4-9043ee056d4d.jpg"></img>


<br>

<img  width="400" height="400" src="https://user-images.githubusercontent.com/87301268/158807236-1ed3b95b-7ac8-405a-bbba-c60a0e48fda9.jpg"></img>
<img  width="400" height="400" src="https://user-images.githubusercontent.com/87301268/158807318-97af8537-dbbc-4f28-a471-f1378c8171aa.jpg"></img>

- 이미지 순서는 `row` , `column`, `row-reverse`, `column-reverse` 순서 입니다.

- `row-reverse`는 이미지 상으로는 순서만 역순으로 보여지지만 실제로는 요소의 위치 또한 왼쪽기준 맨 오른쪽으로 달라 붙습니다.

<br>

## justify-content

- justify-content : 메인축 방향으로 자식을 정렬하는 속성

```css
.flexBox {
	justify-content: flex-start;
	/* justify-content: flex-end; */
	/* justify-content: center; */
	/* justify-content: space-between; */
	/* justify-content: space-around; */
	/* justify-content: space-evenly; */
}

```

![1624477909754-mODpe4NANa--image](https://user-images.githubusercontent.com/87301268/158812743-423be047-4a0d-4eae-81cf-e97bf0d73b35.png)


1. `flex-start (기본값)` : 부모의 자식들을 시작점으로 정렬합니다. <br>
flex-direction이 row(가로 배치)일 때는 왼쪽, column(세로 배치)일 때는 위.

2. `flex-end` : 부모의 자식들을 끝 점으로 정렬합니다. <br>
flex-direction이 row(가로 배치)일 때는 오른쪽, column(세로 배치)일 때는 아래.

3. `center` : 부모의 자식들을 가운데로 정렬합니다.

4. `space-between` : 부모의 자식들 사이(between) 에 균일한 간격을 만들어 줍니다.

5. `space-around` : 가운데 자식 기준 양 옆으로 균일한 간격을 만들어 줍니다.

6. `space-evenly` : 부모의 자식들 사이와 양 끝에 균일한 간격을 만들어 줍니다.

<br>

## align-items

- align-items : 수직축 방향으로 자식을 정렬하는 속성 (하나의 flex라인에 흐르는 cross axis을 기준)
```css
.flexBox {
	align-items: stretch;
	/* align-items: flex-start; */
	/* align-items: flex-end; */
	/* align-items: center; */
	/* align-items: baseline; */
}
```
- 몇 가지 속성을 빼고는, justify-content 에서 설명한 속성과 비슷하기 때문에 그림으로 보는 것이 이해하기 좋습니다.

<img width="376" alt="css-21" src="https://user-images.githubusercontent.com/87301268/158811927-3dc789be-9ab1-4ee2-8ba3-c658e1e2da6c.png">

- `baseline` : 자식들을 텍스트 베이스라인 기준으로 정렬합니다.

<br>

## 전반적으로
- flex 가 무엇인지, 어떻게 사용하는지 정리 하였습니다.

- 위의 속성만 사용해도 어느정도 아이템들에 대한 배치는 가능하다고 생각 합니다.

- 나머지 자세한 속성들은 제 [기술블로그](https://blog.naver.com/x7788/222454346221) 와 아래의 참고 링크들을 활용 하시면 좋을 것 같습니다.


<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222454318709)
- [김버그의 HTML&CSS는 재밌다](https://edu.goorm.io/lecture/20583/%EA%B9%80%EB%B2%84%EA%B7%B8%EC%9D%98-html-css%EB%8A%94-%EC%9E%AC%EB%B0%8C%EB%8B%A4)
- [노마드코더 - 코코아톡 클론코딩](https://nomadcoders.co/kokoa-clone)
- [1분 코딩](https://studiomeal.com/archives/197)
- [MDN, CSS-flex](https://developer.mozilla.org/ko/docs/Web/CSS/flex)
- https://www.samanthaming.com/flexbox30/4-flexbox-axes/
- https://dirask.com/posts/CSS-justify-content-in-flexbox-flex-direction-row-1enA8D