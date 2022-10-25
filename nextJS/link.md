# Next.js - Link
Next.js 에서는 `<a>` 태그를 사용해서 페이지 이동하는 것을 반가워 하지 않습니다.
이유는, 앱 내에서 페이지를 이동 할 때 사용하는 특정 컴포넌트가 존재하기 때문 입니다.


- React 에서 React router link 를 사용하는 이유와 같습니다.
- __`<a>` 태그를 사용해서 페이지를 이동하게 되면 애플리케이션은 새로고침이 발생 합니다.__

![](https://velog.velcdn.com/images/hoho_0815/post/8c6021df-9351-4b53-a334-af95d01c30c0/image.png)


***
<br>

## Link
- 페이지간의 탐색을 __CSR__ 으로 진행 할 수 있습니다.
- __Code Splitting__ 이 페이지 별로 진행 되어 각 페이지에서 필요한 것만 로드할 수 있도록 합니다.
- 특정 페이지에서 오류가 발생하더라도 나머지 앱은 정상 작동 합니다.
- 라우팅 라이브러리가 필요하지 않습니다.


***
<br>

## next/link
가장 먼저 import 를 합니다.
- 별도의 설치는 필요하지 않습니다.
```js
import Link from "next/link";
```

위의 사진에 있던 코드를 알맞게 바꾸면

```js
import Link from "next/link";

const NavBar = () => {
    return (
        <nav>
            <Link href="/home">
                <a>Home</a>
            </Link>
            <a href="/About">About</a>
        </nav>
    );
}

export default NavBar;
```
- 페이지 전환 속도를 비교하기 위해 한 쪽만 바꿨습니다.

![](https://velog.velcdn.com/images/hoho_0815/post/5e8de82b-34f9-4df9-8cb0-6725c1932d9a/image.gif)

***
<br>

## Link의 props정리
1. href ( 필수 ) 
- 이동할 경로 혹은 URL

```js
<Link href="이동할 경로 혹은 URL">
    ...
</Link>
```
<br>

2. as

- 이동할 경로 혹은 URL

```js
<Link href="/" as="브라우저의 주소창에 표시될 URL">
    ...
</Link>
```
<br>

3. passHref

  기본값 : false  

- next Link에서 하위 컴포넌트로 href 속성을 전달해주는 역할

```js
<Link href={{ pathname: "post", query: { id: post_id } }} passHref>
  <ChildATag>A태그</ChildATag> // 자식태그가 styled component a태그인 경우
</Link>

//참고 : https://f-dever-error-log.tistory.com/56
```
 - 위처럼 passHref를 넣어서 작성해야 자식 컴포넌트의 a태그에 href가 전달 됩니다.
 - 더 많은 자료는 참고란에 있습니다.
 
 ***
 <br>
 
## Link 는 Prefetching

`<Link>` 컴포넌트를 이용하면, Viewport에 Link 컴포넌트가 노출되었을때
href 로 연결된 페이지의 chunk를 로드 합니다.

__이를 통해 성능을 최적화 합니다.__
 
***
<br>

## Link Styles
- 본 서비스 외부 링크로 연결 할 때는 `<a>` tag 만 쓰면 됨
- __Link Component__ 에 스타일을 줄 때는`<a>` tag 에 줘야 함

***
<br>

## 참고
- https://nextjs.org/docs/api-reference/next/link
- https://im-designloper.tistory.com/103
- https://nextjs.org/