# Next.js - Data Fetching
데이터 가져오기 
-> 화면에 무엇인가를 그리려면 어디선가 __데이터를 가져와야__ 합니다.

***
<br>

## 1. SSR (Server Side Render)
페이지에 대한 요청이 있을 때마다 서버에서 페이지를 만들어 반환 합니다. 서버에서 매번 연산을 해야하며 캐시를 사용하는 것이 상대적으로 어렵기 때문에 SSG에 비해 느립니다. 하지만 항상 최신의 정보를 보여주어야하는 경우, SSR를 사용하는 것이 좋다. 

<br>

(프론트)서버에서 HTML 파일을 만들어서 보내기 때문에 CSR에 비해 사용자가 더 빠르게 화면을 인식할 수 있습니다. 하지만 이벤트 등 페이지의 동작을 위해서는 hydrate라는 과정을 통해서 JS 코드가 실행되어야 합니다.

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/c338faaf-7026-4003-81da-e374ffbb66bb/image.jpeg)

<br>

- __서버__ 가 그립니다. 
- 그린다 : 데이터를 가져와서 나타낸다.

<br>

> 즉, 서버가 데이터를 가져와서 그린다.

<br>

### 1.2 SSR 을 담당하는 함수
> getServerSideProps()

<br>

Next.js에서 SSR을 사용하기 위해서는 페이지에서 getServerSideProps() 를 통해 데이터를 받아와야 합니다.

- 빌드타임에만 실행되는 getStaticProps()와는 달리 getServerSideProps()는 페이지에 대한 요청이 있을 때마다 실행된다.

<br>

#### 예시 1
```js
function Page({ data }) {
  // 데이터를 통해 페이지 렌더링...
}

export async function getServerSideProps() {
  // 페이지를 요청할 때마다 매번 실행된다.
	// 데이터를 받아온다.
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  // Pass data to the page via props
  return { props: { data } }
}

export default Page
```

<br>

#### 예시 2
```js
export async function getServerSideProps(){
  console.log('server')
  return {
    props : {time : new Date().toISOString()}
  }
}

export default function Home({time}) {
  return (
    <div className={styles.container}>
      {time}
    </div>
  )
}
```
- 위 코드를 실행시키면 터미널에서 __server__ 라는 로그가 지속적으로 발생합니다.
- 이유는 서버에서 그리기 때문 입니다. 
- 화면 콘솔창에는 찍히지 않습니다!!
- 페이지에서 props 를 받아서 자유롭게 사용 할 수 있습니다.

> 즉, __getServerSideProps__ 는 서버에서 데이터를 가져왔고, 
그것을 페이지에다가 props 로 전달한 것 입니다. 

***
<br>

## 2. CSR ( Client Side Render )
Next.js 에서 CSR을 사용하는 경우는 두가지로 나누어볼 수 있다.
<br>

1. url을 입력을 통해 pre-rendering 된 페이지를 받고 useEffect()를 통해 데이터를 추가로 불러오는 경우
2. `<Link/>` 나 router.push()를 통해 페이지 전환이 일어나는 경우이다.

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/bc910d2d-3c19-4379-b905-606331e435d7/image.jpeg)

<br>

- SSR 과 정반대로 __클라이언트__ 가 그립니다.

<br>

> 즉, 클라이언트가 데이터를 가져와서 그립니다.

<br>

### 2.1 CSR 을 권장하는 경우
페이지를 pre-rendering 할 필요가 없거나, 데이터의 업데이트가 자주 일어난다면 CSR을 사용하는 것을 권장 합니다. 예를 들어 유저 대시보드 페이지는 해당 유저만을 위한 비밀 페이지이기 때문에 SEO가 필요하지 않으며 따라서 pre-rendering할 필요도 없습니다. 또한 데이터가 자주 변경되기때문에 CSR이 적합 합니다.


<br>

### 2.2 CSR 을 담당하는 함수
- 따로 존재하지 않습니다.
- 기존 React 사용법과 동일 합니다.
- next.js 의 Link 등...

***
<br>

## 3. SSG ( Static-Site Generation )
빌드시 HTML 파일을 미리 렌더링하는 것이 SSG 입니다. 정적인 페이지에 주로 쓰이며, HTML 파일을 미리 생성하기 때문에 서버에서 매번 연산을 하지 않아도 될 뿐 아니라, 별다른 설정 없이 CDN 캐시 사용이 가능하기 때문에 SSR 보다도 훨씬 빠른 속도를 보여줍니다.

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/174717d3-d0bd-4db8-a090-714c6a30c854/image.jpeg)

<br>

SSG는 Next.js 의 기본적인 렌더링 방식 입니다. 하지만 백엔드 서버로부터 데이터를 받아서 렌더링하는 경우, 페이지에서 useEffect()를 사용한다면 url을 통한 페이지 접근시 SSG로는 데이터가 필요없는 부분만 구성하고, 데이터를 필요로하는 부분은 CSR로 렌더링하게 됩니다.

<br>

페이지에서 데이터를 필요로 하는 부분이 클수록 사용자가 화면을 인식하는데 시간이 오래걸리며 이는 SSG의 장점을 전혀 활용하지 못하게 되는 것을 의미 합니다.

<br>

이는 Next.js 에서 제공하는 페이지 단위 함수 getStaticProps()를 통해 해결할 수 있습니다.

<br>

### 3.1 SSG 를 담당하는 함수
> getStaticProps()

<br>

- yarn dev 를 사용하는 개발환경에서는 제대로 동작하지 않습니다. (SSG 가 SSR 처럼 동작 합니다!)
- 정확하게 확인하려면 빌드를 한 후 yarn start 를 해야 합니다.
- __SSG__ 에서는 새로고침을 해도 페이지가 변함없이 동일 합니다. ( 콘솔조차 찍히지 않습니다. )

<br>

> yarn build 를 할 때 페이지를 미리 만들었다는 뜻 입니다.

<br>

즉, 빌드하는 타이밍에 데이터를 다 가져와서 보여줄 페이지를 static 한 페이지를 미리 생성한 것입니다.

<br>

#### 예시 1
```js
function Blog({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li>{post.title}</li>
      ))}
    </ul>
  )
}

// getStaticProps 함수는 server-side에서 build 타임에만 실행된다.
export async function getStaticProps() {
  // build 타임에 데이터를 받아온다.
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // { props: { posts } }를 반환함으로써, 
  // Blog 컴포넌트는 빌드타임에 props 로 posts를 받을 수 있다.
  return {
    props: {
      posts,
    },
  }
}

export default Blog
```

<br>

- 보이는 바와 같이 getStaticProps는 빌드타임에 서버로부터 데이터를 받아와 반환하고 
- getStaticProps를 사용하는 페이지는 빌드타임에 getStaticProps 로부터 props를 받아올 수 있습니다.
- 따라서 빌드타임에 데이터에 따른 화면 구성이 가능

<br>

#### 예시 2
```js
export async function getStaticProps(){
    console.log('server')

    return {
    props : {time : new Date().toISOString()}
    }
}

export default function ssg({time}) {

    return (
        <div>
            <h1>{time}</h1>
        </div>
    )
}
```

<br>

- 위의 예시처럼 dev 환경에서는 새로고침하면 시간이 계속 변하지만
(SSR 처럼 node에 로그가 찍힙니다.)
- 빌드하고 배포한 후 실행하면 정적인 페이지가 만들어집니다. 
(새로고침해도 시간이 화면의 시간이 변하지 않습니다.)

![](https://velog.velcdn.com/images/hoho_0815/post/36f3492b-43f2-4ca4-9781-66e34c3cffe4/image.gif)

<br>

### 3.2 동적이지도 않고 정적인데 필요한 이유는 ?
- SSR 은 항상 접근 할 때 마다, 즉 새로고침 할 때 마다 서버가 동작 합니다

<br>

> 즉, 데이터를 그릴 때 마다 서버가 동작하고 
여러 사용자가 접근한다고 하면 서버의 부하가 많아집니다.

<br>

- 하지만 __SSG__ 를 사용하면

<br>

> 정적인 페이지 한정이지만, 그 페이지는 누가 접근해도 빌드할 때 이미 만들어져있는 것을 보여주기 때문에 서버의 부하 없이 서비스를 제공 할 수 있습니다.

<br>

### 3.3 SSG 페이지의 데이터가 변경되면 ? : revalidate

빌드 타임에 정적파일을 생성하기 때문에 이후 데이터 변경이 일어나도 페이지에 반영이 되지 않습니다. 

<br>

> 이런 문제를 해결하기 위해 getStaticProps() 에는 revalidate라는 옵션이 존재하며 초 단위로 입력할 수 있다.

<br>

빌드를 통해 페이지가 생성된 후 revalidate 값(초)이 지나기 전 들어오는 요청에 대해서는 기존의 페이지를 반환 합니다. revalidate 값이 지나고 들어오는 최초 요청에 대해서는 기존 페이지를 반환하고 페이지를 새로 빌드 합니다. 이후 들어오는 요청에 대해서는 새로 빌드된 페이지를 반환.(반복) 

<br>

>즉 revalidate에 10을 입력했다면 10초마다 재빌드 되는 것이 아니라, 10초 이후 요청이 새로 들어왔을 때 재빌드 하여 새로운 페이지를 만드는 것 입니다.

<br>

```js
export async function getStaticProps() {
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  return {
    props: {
      posts,
    },
    // Next.js will attempt to re-generate the page:
    // - When a request comes in
    // - At most once every 10 seconds
    revalidate: 10, // 초 단위
  }
}
```

<br>

- 아래 ISR 에서 한번 더 나옵니다.

***
<br>

## 4. ISR ( Incremental Static Regeneration )

- __증분 정적 사이트__를 재생성 합니다.
- 재생성한다 : (특정 주기로) 데이터를 가져와서 다시 그립니다.

<Br>

> 즉, (특정 주기로) 정적인 사이트의 데이터를 가져와서 다시 그린다.

<br>

### 4.1 getStaticProps ( with revalidate )
- revalidate 에 얼마 주기로 재생성 될지를 결정 합니다.
- SSG 처럼 개발환경이 아닌 빌드 후 배포해서 확인 합니다.

<br>

#### isr.js
```js
export async function getStaticProps(){
    console.log('server')

    return {
    props : {time : new Date().toISOString()},
    revalidate : 1, // 1 초
    }
}

export default function isr({time}) {

    return (
        <div>
            <h1>{time}</h1>
        </div>
    )
}
```

<Br>

- 기존 SSG 는 새로고침해도 페이지의 숫자나 값이 동적으로 변하지 않습니다.
- 하지만 ISR 은 설정한 특정 주기대로 화면을 동적으로 변화시킬 수 있습니다.

![](https://velog.velcdn.com/images/hoho_0815/post/66d337d5-717c-4a2d-824c-5c61b93cb21c/image.gif)

- 새로고침을 아무리해도 1초 간격으로 실행이 됩니다.

***
<br>

## 정리 ( Data Fetching )
- 페이지를 그리는 방식 -> 데이터를 가져와서 그린다.
- 3 + 1 -> SSR / CSR / SSG / ISR
- SSG -> yarn dev 로는 SSR 처럼 동작
- 필요에 맞춰서 -> SSR은 서버 부하 / SSG + ISR

***
<br>

## 참고
- https://fastcampus.co.kr/dev_online_nextjs
- https://velog.io/@sj_dev_js/Next.js-%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B1%B0%EC%9D%98-%EB%AA%A8%EB%93%A0-%EA%B2%83
