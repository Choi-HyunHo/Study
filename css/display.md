# display 속성

요소가 화면에 어떻게 드러나게 할지를 결정하는 속성 입니다.<br>
대표적으로 inline, block, inline-block, none

<br>

## inline

- inline은 text크기만큼만 공간을 점유하고 줄바꿈을 하지 않습니다.
- `width`, `height`, `top`, `bottom` 속성은 무시 됩니다.
- `left`, `right` 속성은 사용 가능
- `margin` 은 상하는 적용 되지 않고, 좌우만 가능합니다.

- `padding` 은 다 가질 수 있습니다.

<br>

## block

- block은 한 영역 전체를 차지하는 박스 형태를 가지는 형태
- `width`, `height`, `margin`, `padding` 속성이 모두 반영 됩니다.
- 항상 새로운 라인에서 요소가 시작 합니다.
- 따로 부모의 height를 선언하지 않을 경우, 자식 요소의 `height`의 합 = 부모의 `height`

- 화면 크기의 전체 가로폭을 영역으로 차지 합니다. -> 다른 요소가 위에서 아래 요소가 올라오지 못하게 길막

<br>

## inline-block

- inline-block은 inline속성과 block속성의 특징을 둘다 가지고 있습니다. 즉, inline 엘리먼트 처럼 줄바꿈 없이 다른 엘리먼트과 나란히 배치 됩니다.

- inline속성과 다르게 `width`, `height` 적용 가능하고, `margin` 속성의 상하 간격 지정이 가능 합니다.


<br>

## none

- none: 선택한 요소들을 화면에 나타나지 않게 합니다. 

- visibility: hidden과의 차이점은 영역이 남아있는지 여부가 다릅니다. -> display: none은 존재를 잊게 합니다.

<br>

## 코드
```css
display : block;
display : inline;
```
- 위와 같은 방식으로 적용 할 수 있습니다.

<br>

## 참고
- https://ofcourse.kr/css-course/display-%EC%86%8D%EC%84%B1
- https://programming119.tistory.com/97
- [코드 안식처](https://blog.naver.com/x7788/222450599207)