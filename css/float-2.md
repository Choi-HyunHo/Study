# Float 사용법

float 만 사용하게 되면 레이아웃이 망가지기 때문에 알맞게 수정을 해야 합니다.

<br>

## overflow : hidden

- 자식에게 float를 주면 부모는 자식을 잃어버리고 높이 또한 잃어버리게 됩니다.

- 하지만 float 된 자식을 가지고 있는 부모에게 `overflow : hidden`을 하면 부모가 집 나간 자식을 찾을 수 있습니다.

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
*{
    box-sizing: border-box;
    margin: 0;  
}

.parent{
    width: 400px;
    margin: 0 auto;
    background-color: #eff2f7;
    overflow: hidden;
}

.child{
    width: 200px;
    height: 200px;
    text-align: center;
    color: #fff;
    font-weight: 600;
    float: left;
}

.red{
    background-color: red;
}

.yellow{
    background-color: yellow;
    
}

.blue{
    background-color: blue;
}

.other{
    background-color: black;
    color: white;
}
```

![bandicam 2022-03-19 16-12-13-772](https://user-images.githubusercontent.com/87301268/159111546-fe2f2cd2-6c35-468a-a4ef-1f6b116252ad.jpg)

- 부모의 높이가 다시 찾아진걸 볼 수 있습니다.

<br>

## clearfix
- float로 망가진 레이아웃을 고치기 위해 만들어졌습니다.

- left | right | both 를 가질 수 있습니다.

1.  `clear: left` 는 앞으로 float left 된 녀석들이 앞에 있다면 그 녀석들의 위치를 정확하게 파악을 해서 영향을 받지 않지 않습니다.
2.  `clear: right` 는 float right로 부터 영향을 받지 않습니다.
3.  `clear: both` 는 float left, float right 둘다 영향을 받지 않습니다.

<br>


![image](https://user-images.githubusercontent.com/87301268/159111950-a859b802-0fa0-488a-8a18-8ba901ff75ab.png)

- A와 B 둘 다 block 요소 입니다. A에게 float: left를 주면 A는 집을 나가게 되고 뒤에 있는 B가 A가 존재 하지 않는다고 생각해 위로 올라가게 되는 상황 입니다.

<br>

![image (1)](https://user-images.githubusercontent.com/87301268/159111992-29f1595b-e346-4587-b27d-b87b6ea84398.png)

- 이 상황에서 B에게 clear: left를 주면 float이 어디 있는지를 파악할 수 있는 능력을 주면서 보지 못했던 A의 존재를 알게 되고 B는 A의 아래로 배치가 됩니다.

- 결론은, 부모 입장에서는 B가 float 되지 않아서 부모는 B의 위치랑 사이즈에 대해서 제대로 파악 할 수 있습니다. <br>
부모는 별도의 height가 없는 경우 자식들의 `height`의 합을 자신의 `height`로 한다고 했으니 A는 여전히 집을 나갔기 때문에 어디로 갔는지 알 길이 없지만<br>
 clear가 된 B 기준으로 올바르게 전체 자식의 새로 영역을 파악 할 수 있습니다.

<br>

## 가상 요소(Pseudo-Element) 와 함께 사용하기

__실제로 HTML이 존재하지 않지만 CSS에서 `fake` 요소를 만들어서 `fake 요소` 에게 clear 를 줍니다.__ 
<br>

이렇게 사용하면 굳이 스타일을 바꾸면서 까지 HTML 마크업을 해야 할 이유가 없습니다.

![image (2)](https://user-images.githubusercontent.com/87301268/159112449-76a35a1f-ffdd-4cbe-8b14-462f3193cc56.png)
<br>
![image (3)](https://user-images.githubusercontent.com/87301268/159112479-b04ee6f0-769e-4dd6-b764-2f58ecfad19f.png)

- 가상요소는 각 요소당 무조건 두개씩 만들 수 있습니다.
- `::before( 맨 앞 )` , `::after( 맨 뒤 )`

- 대신, 몇 가지 규칙을 지켜야 합니다.
    - 가상요소를 사용할 때 반드시 `content` 를 써야 합니다.
    - clear 는 반드시 `display: block` 한테만 사용 할 수 있습니다.

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
*{
    box-sizing: border-box;
    margin: 0;  
}

.parent{
    width: 400px;
    margin: 0 auto;
    background-color: #eff2f7;
    overflow: hidden;
}

.parent::after {
	content: "";
    display: block;
    clear: both;
    /* left | right | both */
}

.child{
    width: 200px;
    height: 200px;
    text-align: center;
    color: #fff;
    font-weight: 600;
    float: left;
}

.red{
    background-color: red;
}

.yellow{
    background-color: yellow;
    
}

.blue{
    background-color: blue;
}

.other{
    background-color: black;
    color: white;
}
```
- 코드의 결과는 맨 처음 `overflow : hidden` 마찬가지로 동일하게 나옵니다.

<br>

## 정리

- 가로 배치를 해야 할 것들을 먼저 파악
- float를 선언한 요소들의 부모를 찾아 clearfix를 할 것

- 범용적으로 사용할 경우
```css
.clearfix::after{
    content: ""; 
    display : block;
    clear : /* left | right | both */ ;
}
```

<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222454318709)
- [김버그의 HTML&CSS는 재밌다](https://edu.goorm.io/lecture/20583/%EA%B9%80%EB%B2%84%EA%B7%B8%EC%9D%98-html-css%EB%8A%94-%EC%9E%AC%EB%B0%8C%EB%8B%A4)