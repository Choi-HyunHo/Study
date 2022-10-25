# Next.js - Router 와 Link 차이

원래 페이지 전환을 할 때 a태그를 사용하지만, 전체 새로고침이 되며 다시 정보들을 새롭게 가져옵니다. 이는 흰 화면이 보일 수도 있고 매번 통신을 새롭게 하다 보니 사용자 경험 측면에서도 떨어집니다.

***
<br>

## Link
- Link 태그에 href 속성을 추가하여 가고자 하는 주소를 넣습니다. (a 태그의 href와 동일합니다.)
- 안쪽에는 a 태그로 감싸줍니다.
- className과 같은 속성들을 a 태그에 추가합니다.
- 페이지 새로고침 없이 페이지가 전환됩니다.

> Link는 Client-side navigation으로 자바스크립트로 페이지 전환이 이루어집니다.
기본 navigation보다 빠르며 SPA의 특성을 유지합니다.


***
<br>

## Router
react-router-dom의 history.push()와 유사합니다.

- 크롤러가 링크를 감지하지 못하여 SEO가 좋지 않을 수도 있습니다.
- 외부 URL을 사용할 경우 window.location 혹은 a 태그를 사용해야 합니다.

1. router.push
- 페이지가 이동되며 히스토리 스택이 쌓입니다.
- Main -> Login -> List에서 마이페이지로 push 하면 Main -> Login -> List -> Mypage

<br>

2. router.replace
- 페이지가 이동되며 히스토리 스택이 교체됩니다.
- Main -> Login -> List 에서 마이페이지로 replace 하면 Main -> Login -> Mypage

<br>

3. router.back
- 뒤로 가는 기능이며 "window.history.back()"과 동일합니다.

<br>

4. router.reload
- 새로고침 기능이며 "window.location.reload()"와 동일합니다.

<br>

> `<Link>`는 클릭 시 바로 페이지가 전환되지만 라우터는 로직을 처리 후 원하는 시점에서 전환이 가능합니다. ( 핵심 )


***
<br>

정리

- router.push는 onClick에 사용되는 행동(action) 이기 때문에 Link 태그보다 검색에 불리 합니다.
- NextJs의 장점이 SEO (search engine optimization : 검색엔진최적화) 을 원한다면 Link 사용을 추천 합니다.

***
<br>

## 참고
- https://talkwithcode.tistory.com/98
- https://izizi.tistory.com/27