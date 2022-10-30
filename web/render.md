# 브라우저 렌더링 과정
### requests / response -> loading -> scripting -> rendering -> layout -> printing

***
<br>

## requests/response
브라우저가 서버에게 html 파일을 요청, 서버에 request를 하면 가장먼저 index.html을 받아오고 그 파일안에서 링크된 필요한 파일들(CSS,JS)을 받아오고, 필요한 리소스(이미지, 폰트)들을 순차적으로 서버에서 받아옵니다.

<br>

## loading
html 파일을 서버에서 받아 로딩 하고

<br>

## scripting 
html 파일을 읽어 DOM 요소로 변환(DOM 트리 생성), CSS 스타일을 모두 계산해서 CSSOM 트리 생성 합니다.

<br>

## rendering
DOM 트리 + CSSOM 트리 → 브라우저에 표기될 요소들만 선별하여 Render Tree 생성 합니다.

![](https://velog.velcdn.com/images/hoho_0815/post/f7020753-6e5b-43d1-bac7-1b94fd54d1fb/image.png)


<br>


## layout 
Render Tree 각 요소들의 위치와 크기 계산 하고


<br>

## painting
화면에 그림을 그립니다.

***
<br>

## Construction part

html 파일을 브라우저가 이해할 수 있도록 브라우저의 언어로 바꾸는 시점
- DOM, CSSOM, Render Tree를 만드는 부분


<br>

## operation part
Construction part 에서 생성된 Render Tree를 이용해서 브라우저 window에 표기할 요소의 위치, 크기 등을 계산하고 그림을 그리는 시점

- layout, paint, composition을 거쳐서 사용자에게 보여주는 부분

***
<br>

## 조금더 구체적으로는
### paint 

- layout 에서 계산한 요소들을 바로 브라우저에 표시하는 것이 아니라 요소들을 어떻게 배치 했느냐에 따라 요소들을 조금씩 나누어서 이미지를 준비합니다.
- 즉 레이어별로 요소들을 준비만. -> 브라우저의 성능 개선을 위해서
- 브라우저의 한 부분만을 움직이거나 변환할 때 웹 페이지 전체를 다시 업데이트 하는 것이 아니라 특정 레이어만 업데이트 하면 됩니다.
- 그래서 CSS에 `will-change` 라는 속성 값을 사용 합니다.

    - will-change 는 브라우저 요소의 opacity 값이 변경 될 수 있음을 미리 알려서 새로운 레이어에 추가할 수 있도록 합니다. 그러나 많이 쓰면 레어어가 많이 만들어지기 때문에 꼭 필요할 때만 사용해야 합니다.

<br>

### composition 
- 준비된 레이어를 브라우저 위에 순서대로 표시합니다.

***
<br>

## construction part에서 render tree를 빠르게 하는 방법

- DOM 요소와 CSS 규칙이 적을수록 빠릅니다.
- 불필요한 태그의 자제, div 클래스 남용, 쓸데없는 wrapping class나 요소 사용을 자제합니다.

​

## operation part에서 빠르게 하는 방법

- 사용자가 클릭을 해서 요소를 움직이거나 애니메이션을 쓸 때, paint가 자주 발생하지 않도록 만드는 것이 중요 합니다.
예시 ) box를 이동할 때 top,left 등의 속성을 사용하면 paint 뿐만 아니라 layout부터 다시 발생 합니다.

- 하지만, translate을 사용하게 되면 composition 만 일어납니다.

***
<br>

## 참고
- https://medium.com/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98%ED%92%88%EA%B2%A9/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-5c01c4158ce
- 드림코딩