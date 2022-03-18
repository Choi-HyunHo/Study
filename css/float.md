# Float

grid, flex와 마찬가지로 요소를 배치하기 위한 속성 입니다. <br>
float 라는 단어는 원래 ‘뜨다’ 라는 의미이며, `block` 요소들을 가로 배치하기 위해 사용 합니다. <br>
<br>
__하지만 무작정 사용하면 많은 문제가 생길 수 있습니다.__

<br>

## float를 사용하면 문제점 ?

1. 부모가 가지고 있던 자식이 float가 되버리면 집 나간 자식이 되버립니다.
   
    - 부모의 height를 선언하지 않을 경우, 자식요소의 height 의 합 = 부모의 height
   
    - 나간 자식의 자리를 빈 공간(붕 뜨게 된다)으로 인지하여 공간을 채우기 위해 이동 합니다. -> 부모의 height가 줄어듭니다.

```html
<div class="parent">
    <div class="child red">child</div>
    <div class="child yellow">child</div>
    <div class="child blue">child</div>
</div>
```
```css
*{
    box-sizing: border-box;
    margin: 0;  
}

.parent{
    width: 200px;
    margin: 0 auto;
    background-color: #eff2f7;
}

.child{
    width: 200px;
    height: 200px;
    text-align: center;
    color: #fff;
    font-weight: 600;
    font-size: 60px;
}

.red{
    background-color: red;
    float: left;
}

.yellow{
    background-color: yellow;
    
}

.blue{
    background-color: blue;
}
```
![bandicam 2022-03-17 22-21-48-335](https://user-images.githubusercontent.com/87301268/158817600-63dc2358-3681-4e89-b7cc-8fa1325a5597.jpg)
![bandicam 2022-03-17 22-22-11-907](https://user-images.githubusercontent.com/87301268/158817704-99105262-5bf3-42bf-98e8-6abbc64a9a30.jpg)

- 첫 번째는 `float` 를 사용하기 전

- 두 번째는 `red` 에게 `float : left` 선언

- `red` 는 `parent` 라는 부모를 가지고 있었는데 `float`를 선언함 과 동시에 집 나간 자식이 되어 버렸고 즉 요소가 붕 떠있는 상태이고, <br>뒤에 자식들은 앞선 `red`의 자리를 빈공간으로 인식하고 자리가 비워져있어 앞으로 채워진다. -> 따라서 부모의 `height` 또한 변합니다. 

<br>

2. `block` 으로 신분 상승
- `inline`,`inline-block`,`block` 요소들에게 `float` 를 사용하면 -> `block` 으로 속성이 바뀝니다.

- 즉, float가 선언된 요소에게 `block` 의 display 값을 가지게 되므로 -> `margin`, `width`,`height` 값 등을 사용 할 수 있습니다.

<br>

3. `block` 요소가 되지만 길막을 하지는 못합니다.
- 기본적으로 width를 선언하지 않은 경우, width = 부모의 content-box의 100% 를 가지게 됩니다.

- 하지만, `float`가 되면 그렇지 않습니다.
- 위의 코드에서 `width` 값만 지웠습니다.
```css
.child{
    height: 200px;
    text-align: center;
    color: #fff;
    font-weight: 600;
    font-size: 60px;
}
```
![bandicam 2022-03-17 22-39-10-741](https://user-images.githubusercontent.com/87301268/158820743-ba575dbb-a531-4bd7-a1d6-ca4cc7b01342.jpg)

- float를 하면 실제 content가 가진 크기 만큼 붕 뜨게 됩니다.
- 하지만 `block` 이지만, 옆에 다른 요소가 오지 못하게 할 수는 없습니다.

<br>

4. float는 요소는 `inline` 만 찾을 수 있습니다.
- `block` 요소들은 float 를 없는 걸로 취급을 합니다. 하지만 `inline`의 성격을 가진 것은 float의 존재를 기억 합니다.
```html
<div class="parent">
    <div class="child red">child</div>
    <div class="child yellow">child</div>
    <div class="child blue">child</div>
</div>
<div class="other">
    더미 text
</div>
```
```css
.child{
    width: 200px;
    height: 200px;
    text-align: center;
    color: #fff;
    font-weight: 600;
    float: left;
}
```
![bandicam 2022-03-17 23-04-10-878](https://user-images.githubusercontent.com/87301268/158825495-ba57cd31-f056-4417-a85b-cc4fe2e3f35b.jpg)

- 가장 먼저 설명한 "나간 자식의 자리를 빈 공간(붕 뜨게 된다)으로 인지하여 공간을 채우기 위해 이동 합니다. -> 부모의 height가 줄어듭니다."

- 현재 `other` 앞에 있는 `parent`의 `height`가 0 입니다. -> parent 의 자식인 child 에게 float를 선언했기 때문
- 그럼 `other` 는 빈 공간이 되기 때문에 올라가는 것은 자연스러운 현상 입니다.
- 하지만 `other` 안에 있는 text 들은 레이아웃이 깨져버립니다.
- 다시 한번 `blue`에게 `margin-right : 300px` 을 주겠습니다.

![bandicam 2022-03-17 23-13-04-795](https://user-images.githubusercontent.com/87301268/158826161-f54c136a-1484-4ce5-b8ea-defcd5437e7b.jpg)

- 이처럼 `block` 요소들은 찾을 수 없지만 text나 img 같은 `lnline`의 성격을 가진 속성들은 `float`의 존재를 찾을 수 있어서 레이아웃에 영향을 받습니다.


<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222454318709)
- [김버그의 HTML&CSS는 재밌다](https://edu.goorm.io/lecture/20583/%EA%B9%80%EB%B2%84%EA%B7%B8%EC%9D%98-html-css%EB%8A%94-%EC%9E%AC%EB%B0%8C%EB%8B%A4)