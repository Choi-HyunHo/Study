# Box Model

CSS 박스 모델은 HTML Element가 웹페이지에서 차지하는 공간을 정의한 모델 입니다.

<br>

## Box Model 이미지
![슬라이드1](https://user-images.githubusercontent.com/87301268/158051751-72b55b66-cc47-4c6c-bc3b-39d4ef1c3204.jpg)

- Element의 내용이 담긴 `Content` 영역
- Element를 감싸는 경계 `Border` 영역
- Border와 Content 영역 사이의 `Padding` 영역

- Border 바깥의 `Margin` 영역

<br>

# box-sizing

box-sizing 속성을 사용하면, `width` 와 `height` 이 컨텐츠 영역 기준인지, 테두리 영역 기준인지 정할 수 있습니다.

<br>

## box-sizing 종류
- box-sizing 속성을 사용하면, `width` 와 `height` 이 컨텐츠 영역 기준인지, 테두리 영역 기준인지 정할 수 있습니다.
    - box-sizing: `content-box`
        - 기본값이며 컨텐츠 영역 기준. Padding 영역부터 포함하지 않습니다.
   
    - box-sizing: `border-box`
        - 테두리 영역 기준. Margin 영역부터 포함하지 않습니다.

<br>

## 너비와 높이가 480인 정사각형을 만들 때
<br>

### box-sizing: `content-box` 사용

![bandicam_2021-07-29_15-54-26-814](https://user-images.githubusercontent.com/87301268/158052321-df31ea0a-7301-4684-937c-d17686f8d8c1.jpg)

- 정사각형을 만들기 위해 width와 height 둘 다 480px을 주었습니다.
- 사진을 보면 왼쪽 하단 아래 530x520 으로 만들어진 것을 볼 수 있습니다.
- 즉, 박스 전체가 아니라 content 박스 만의 width, height가 설정 되었습니다. 

- 결론 : 우리가 원하는 480px의 정사각형을 만들기 위해서는 일일이 빼고 더하고 연산을 해야 합니다.

<br>

### box-sizing: `border-box` 사용

![bandicam_2021-07-29_15-56-44-894](https://user-images.githubusercontent.com/87301268/158052470-340f36c9-bd7d-4b6b-ada3-08601202f514.jpg)

- 더 이상 content 박스의 가로,세로가 아니라 border 박스가 기준이 됩니다.

- 즉, content와 padding, border까지 포함한 영역이 됩니다.

<br>

## CSS 작업 시
```css
* {
    box-sizing : border-box;
}
```
- 최상단에 먼저 선언을 하고 작업하면 평소 처럼 생각하는 테두리 기준으로 생각 할 수 있습니다.

<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222449570715)
- [MDN, CSS 기본 박스 모델 입문](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)
- [CSS 박스 모델과 box-sizing 속성](https://www.daleseo.com/css-box-model/)
- [CSS / box-sizing / 박스의 크기를 어떤 것을 기준으로 계산할지를 정하는 속성](https://www.codingfactory.net/10630)