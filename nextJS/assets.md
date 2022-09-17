# Next.js - Assets, Meta data, CSS
## 1. Assets
### 1.1 public 디렉토리
- __정적 데이터__를 제공하는 경우 최상위 디렉토리의 __public__ 를 이용하면 됩니다.
- 이미지 경로는 __public__ 폴더가 __/__ 로 매칭 됩니다.
```js
<img src="/images/profile.jpg" alt="Your Name" />
```

<br>

### 1.2 최적화 되지 않은 이미지
```js
<img src="/images/profile.jpg" alt="Your Name" />
```

그러나, 일반 HTML 의 `img` 를 사용하면 아래의 상황들을 수동으로 처리해야 합니다.
- 다양한 화면 크기에 이미지가 __반응형__으로 적용되는지
- 다른 툴 또는 라이브러리로 __이미지 최적화__
- 뷰포트에 들어갈 때만 이미지 로드 등 ...

하지만, Next.js 는 `Image` 를 처리하기 위해 즉시 사용 할 수 있는 구성 요소를 제공 합니다.

<br>

### 1.3  Image Component - 이미지 자동 최적화
__Next.js 는 기본적으로 이미지 최적화도 지원 합니다.__
- 이미지 크기 조정, 최적화와 최신 이미지 포멧 (WebP) 를 제공
- 작은 viewport 의 기기에서 큰 이미지를 불러오는 것을 방지
- 자동으로 이미지 포맷을 선택하고 브라우저가 지원 가능한 포맷으로 이미지를 제공
- 이미지는 기본적으로 __Lazy Loading( 지연 로딩 )__ 됩니다.
-> 즉, 뷰포트 밖의 이미지는 페이지 속도에 영향을 주지 않습니다.
-> __이미지는 뷰포트로 스크롤 될 때 로드 됩니다.__

<br>

> Next.js는 빌드 시 이미지를 최적화하는 대신 사용자가 요청할 때  on-demand 로 이미지를 최적화합니다. 정적 사이트 생성기 및 정적 전용 솔루션과 달리 10개의 이미지를 배송하든 1천만 개의 이미지를 배송하든 빌드 시간이 증가하지 않습니다.

<br>

```js
import Image from 'next/image';

const YourComponent = () => (
  <Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
);
```

- https://nextjs.org/docs/api-reference/next/image

<br>

### 1.4 비교
`<img src = "/images/profile.jpg" alt="1" />`

vs

`<Image src ="/images/profile.jpg" width={144} height={144} alt="1"/>`

__차이점은 ??__

### Next.js 에서 제공하는 Image 를 활용하면,
- 기존 이미지 파일 형식이 완전히 달라집니다.
-> webp 라는 이미지의 포맷인데 jpg 보다 훨씬 더 가볍게, 동일 혹은 더 좋은 해상도의 이미지를 제공하는 포맷 입니다.
- 이미지 Lazy load 기능
-> 처음에 모든 리소스를 받지 않고, 보이는 시점에서 이미지를 받아옵니다.
![](https://velog.velcdn.com/images/hoho_0815/post/06fa9fa0-6af2-4736-bb53-a3367dceee0b/image.gif)

> 궁극적인 목표는 __CLS ( Cumulative Layout Shift )__ : 누적 레이아웃 이동
컴포넌트가 없다가 생기거나, 사이즈가 바뀌거나, 컴포넌트의 변경으로 인해 Dom Tree 에있는 요소들을 다시 렌더링 되는 것을 최대한 줄이는 것

***
<br>

## 2. Meta data
`<title>` HTML 태그 와 같은 페이지의 메타데이터를 수정 하는 방법
- __Head 컴포넌트__ 를 제공 합니다.

```js
import Head from 'next/head'

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta name="viewport" content="initial-scale=1.0, width=device-width" />
      </Head>
      <p>Hello world!</p>
    </div>
  )
}

export default IndexPage
```
<br>

- 태그가 중복되지 않도록 `key` 를 사용하면 다음 예시와 같이 태그가 한 번만 렌더링 됩니다.
```js
import Head from 'next/head'

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta property="og:title" content="My page title" key="title" />
      </Head>
      <Head>
        <meta property="og:title" content="My new title" key="title" />
      </Head>
      <p>Hello world!</p>
    </div>
  )
}

export default IndexPage
```
- 이 경우 두 번째만 <meta property="og:title" /> 렌더링 됩니다. 
- https://nextjs.org/docs/api-reference/next/head

***
<br>

## 3. CSS
Next.js 는 CSS 및 Sass 가 내장 되어 있습니다.

<br>

### 3.1 styled-jsx
Next.js 에 내장 되어 있습니다.

#### 기본 문법
```js
<>
<style jsx>{`
  …
`}</style>
</>
```

```js
<>
  <div className="msg">Hello, World!</div>
  // `(템플릿 문자열)으로 감싸진 사실에 유의하세요!
  <style jsx>`
    .msg {
      font-size: 20px;
    }
  `</style>
</>
```

![](https://velog.velcdn.com/images/hoho_0815/post/a27fb7f1-4429-40d9-8f2b-77144413b237/image.png)

> 기본적으로 styled-jsx는 사용된 컴포넌트에만 영향을 미치며, 
스타일을 전역으로 활용하기 위해서는 global 이라는 속성을 사용합니다.

```js
<div>
  <div className="msg">Hello, World!</div>
  // 스타일 태그에 global 속성을 추가하면
  // 다른 컴포넌트의 .msg 요소에도 스타일을 입힐 수 있습니다.
  <style jsx global>`
    .msg {
      font-size: 20px;
    }
  `</style>
</div>
```

<br>

### 3.2 스타일 객체
```js
import css from "styled-jsx/css";

const style = css`
  .top-wrap {
    background-color: white;
    height: 60px;
  }
`;
export default function Top() {
        return (
          <>
            <div className="top-wrap">
            </div>
            <style jsx>{style}</style>
          </>
        );
```

### 3.3 스타일 바인딩
```js
 export default function Top() {
   		const [colorBind, setColorBind] = useState(true);
        return (
          <>
            <div className="top-wrap">
            </div>
            <style jsx>{`
              .top-wrap {
                background-color: ${colorBind? "red": "blue"};
              }
            `}</style>
          </>
        );
```
- 동적으로 스타일 바인딩이 가능 합니다.
<br>

### 3.4 CSS 작성 및 import
Next.js 는 CSS 와 Sass 가 내장되어 있어서, `.css` 나 `.scss` 를 __import__ 할 수 있습니다.
- 모듈로 스타일링 파일을 관리 할 수 있습니다.
- Code Splitting 는 CSS 모듈에서도 적용 됩니다.

> __모듈__
-> __프로그램의 기능을 독립적인 부품으로 분리한 것__ 을 모듈이라고 한다. 일반적으로 서브루틴과 데이터 구조의 집합체로서, 그 자체로서 컴파일 가능한 단위이며, 재사용 가능하고 동시에 여러 다른 모듈의 개발에 사용될 수 있다. 
출처 : 네이버

즉, <span style="color : #0000FF"> 특정 기능별로 나누어지는 프로그램 덩어리 </span> 라고 생각 할 수 있습니다.

`components/layout.module.css`
```js
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```
<br>

`components/layoust.js`
```js
import styles from './layout.module.css'

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

***
<br>


## 참고
- https://velog.io/@dasu/Next.js-styled-jsx-%EB%A1%9C-component-style-%EC%A0%81%EC%9A%A9
- https://merrily-code.tistory.com/56
- https://nextjs.org/learn/basics/assets-metadata-css/css-styling