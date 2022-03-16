i# Grid

2차원적으로 수평과 수직을 동시에 나눌 수 있는 레이아웃 방법 입니다.

<br>

## display : grid , column 과 rows

```html
<div class="father"> 
    <div class="child">1</div>
    <div class="child">2</div>
    <div class="child">3</div>
    <div class="child">4</div>
    <div class="child">4</div>
    <div class="child">4</div>
    <div class="child">4</div>
    <div class="child">4</div>
</div>
```
```css
.father{
    display: grid; /* 자식의 부모에게 선언 */
    grid-template-columns:250px 250px 250px; /* 원하는 열만큼 입력 한다 */
    grid-template-rows: 100px 80px 300px; /*원하는 row의 크기를 입력 */
    column-gap: 10px; /* column 간의 차이 */
    row-gap:  10px; 
}

.child{
    flex-basis: 300px;
    background: peru;
    color: white;
    font-size: 50px;
}
```
![412](https://user-images.githubusercontent.com/87301268/158549535-5ef85ad4-d977-45af-8a24-1b5cc3c17d53.jpg)

<br>

## repeat
```css
.grid{
    display: grid;
    grid-template-columns: 200px 200px 200px 200px;
    grid-template-rows: 300px 300px 300px 300px;
}
```
- 매번 반복하면서 만들면 오래걸리고 효율적이지 않습니다.

- 그래서, repeat을 사용하여 빠르고 간편하게 나타낼 수 있습니다.

```css
.grid{
    display: grid;
    grid-template-columns: repeat(4, 200px);
    grid-template-rows: repeat(4, 300px);
}
```

<br>

##  grid-template-areas

- 마음대로 grid 템플릿을 디자인 할 수 있습니다.

```css
.grid{
    display: grid;
    grid-template-columns: repeat(4, 200px);
    grid-template-rows: 100px repeat(2, 200px) 100px;
    grid-template-areas: 
    "header header header header"
    "content content content nav"
    "content content content nav"
    "footer footer footer footer";
}

.header{
    background-color: #2ecc71;
    grid-area: header; 
}

.content{
    background-color: #3498db;
    grid-area: content;
}

.nav{
    background-color: #8e44ad;
    grid-area: nav;
}

.footer{
    background-color: peru;
    grid-area: footer;
}
```
- 주의할 점은 `grid-area`를 통해 `header`가 무엇인지, `content`가 무엇인지, `footer`가 무엇인지 요소에게 선언해야 합니다.

![45](https://user-images.githubusercontent.com/87301268/158550706-fd6bf0f9-158d-42bb-a582-926d5366d59e.jpg)

<br>

## Rows and Columns

1. grid-column-star
2. grid-column-end
3. grid-row-start

4. gird-row-end

### 특징 
- grid-template-ares와 비슷한 기능을 하는 속성 입니다.
- column과 row가 아닌 줄 의 시작과 끝을 정합니다.

- 그림을 보면서 이해 하면 더 쉽습니다.

```html
<div class="grid">
        <div class="header"></div>
        <div class="content"></div>
        <div class="nav"></div>
        <div class="footer"></div>
</div>
```
```css
.grid{
    display: grid;
    grid-template-columns: repeat(4, 100px);
    grid-template-rows:  repeat(4, 100px);
}

.header{
    background-color: #2ecc71;
    grid-column-start: 1;
    grid-column-end: 3;
}
```
<img align="left"  src="https://user-images.githubusercontent.com/87301268/158552005-d7f0156d-33f9-44ee-9d70-ce07b123048d.jpg"></img>


<img align="center" src="https://user-images.githubusercontent.com/87301268/158552127-984620b8-55bf-4800-8b74-0640cf733116.jpg"></img>


<br>

```css
.grid{
    display: grid;
    grid-template-columns: repeat(4, 100px);
    grid-template-rows:  repeat(4, 100px);
}

.header{
    background-color: #2ecc71;
    grid-column-start: 1;
    grid-column-end: 5;
}

.content{
    background-color: #3498db;
    grid-column-start: 1;
    grid-column-end: 4;
    grid-row-start: 2;
    grid-row-end: 4;
}

.nav{
    background-color: #8e44ad;
   
}

.footer{
    background-color: peru;
}
```
![gird2](https://user-images.githubusercontent.com/87301268/158554141-c2672ffc-0320-4a0b-9378-80a1c0c9346d.jpg)

- `content` 를 기준으로 `grid-row` 를 보면 됩니다.(파란색)

<br>

## 전반적으로
- grid 가 무엇인지, 어떻게 사용하는지 정리 하였습니다.

- `flexbox`의 `justify-items & align-items` 처럼 item 들을 이동 시킬 수 있습니다.
- 또한 `justify-content & align-content` 속성을 사용하여 grid 전체의 위치를 이동 할 수 있습니다. 

    - item은 셀 하나하나, content는 grid 전체

<br>

- grid 에 대한 다양한 속성의 사용법은 제 [기술 블로그](https://blog.naver.com/x7788/222577489812)에 정리 하였습니다.

<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222577489812)
- [노마드 코더, CSS layout 마스터 클래스](https://nomadcoders.co/css-layout-masterclass/lobby)