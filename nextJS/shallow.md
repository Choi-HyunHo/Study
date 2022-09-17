# Dynamic Routes
[slug] 값은 어떻게 활용할 것 인가? - pages/category/[slug].js

- __slug__ 값에 따라서 우리가 원하는 페이지가 다르게 보여져야 합니다.

<br>

## 1. useRouter
- Next.js 에서 제공하고 있는 라이브러리

> import { useRouter } from 'next/router' // 선언
const router = useRouter(); 
const { slug } = router.query;

<br>

### 1.1 category.js
```js
import { useRouter } from "next/router";

const CategorySlug = () => {
    const router = useRouter()
    const { slug } = router.query

    return (
        <h1>Category {slug}</h1>
    )
}

export default CategorySlug;
```
![](https://velog.velcdn.com/images/hoho_0815/post/5fd44d36-36f5-49a8-9d7c-396d9603d792/image.gif)


> __query__ 가 있다면 ?
/category/food?from=event
{"slug" : "food", "from" : "event" }

***
<br>

## 2. 다중 슬러그
### 2.1
- pages/[username]/[info].js
- const { username, info } = router.query

<br>

### 2.1.2 폴더 구조
![](https://velog.velcdn.com/images/hoho_0815/post/2a56ef8e-20be-4c6a-8aed-329835a2f84d/image.png)

<br>

### 2.1.3 info.js
```js
import { useRouter } from "next/router";

const userInfo = () => {
    const router = useRouter()
    const { username, info } = router.query

    return (
        <h1>Username's {username} {info}</h1>
    )
}

export default userInfo;
```
![](https://velog.velcdn.com/images/hoho_0815/post/87d06d95-e84f-400f-a834-af229f8cd0d0/image.gif)

***
<br>

## 3. slug 는 배열
- pages/cart/[...slug].js
- const { slug } = router.query

<br>

### 3.1.1 폴더 구조
![](https://velog.velcdn.com/images/hoho_0815/post/0800c32f-38a0-4e2b-a271-dacac5a372eb/image.png)
![](https://velog.velcdn.com/images/hoho_0815/post/b324f38a-4394-4325-bb71-97f8e26ecb95/image.png)

***

## 4. 옵셔널 slug
- 저번 포스팅 처럼 `pages/cart/[...slug].js` 를 __/cart__ 로 접근하면 404가 나옵니다.
- 이유는 해당 폴더의 index.js 가 없기 때문입니다.
- 하지만 `pages/cart/[[...slug]].js` 해주면 존재하지 않아도 기본 페이지가 생성 됩니다.

<br>

### 4.1 폴더 구조
![](https://velog.velcdn.com/images/hoho_0815/post/0b651689-b1c1-4947-acd4-2c89eb82d9d1/image.png)
![](https://velog.velcdn.com/images/hoho_0815/post/fd40671d-3946-428d-b81d-afabee651156/image.png)

***

## 5. router.push
router 를 사용하면서 페이지 이동이 가능 합니다.

### 5.1 코드
```js
import Link from "next/link";
import { useRouter } from "next/router";

const CartSlug = () => {
    const router = useRouter()
    const { data } = router.query

    return (
        <>
            <h1>CartSlug {JSON.stringify(data)}</h1>
            <Link href="/cart/2022/08/07">
                <a>2022년 8월 7일로</a>
            </Link>
            <button onClick={() => router.push('/cart/2022/08/10')}>2022년 8월 10일로</button>
        </>
    )
}

export default CartSlug;
```

![](https://velog.velcdn.com/images/hoho_0815/post/9c2f2df7-7221-4e52-9ed4-6e235e88c273/image.gif)

***

## 6. Shallow Routing
- Next.js 에서 페이지를 그릴 때는 pre-render 를 우선적으로 고려 합니다.
> getServerSideProps / getStaticProps 등을 다시 실행시키지 않고,
현재 상태를 잃지 않고 __url__ 을 바꾸는 방법

예를 들어, 상태는 유지하면서 url 만 바꾸고 싶은 경우 ?
- 사용자가 어떤 동작을 했고, 그 기록을 __query__ 로 남기고 싶을 때
-> __query__ 로 남기면 사용자가 새로고침을 해도 유지 됩니다.

<br>

### 6.1 url 을 바꾸는 3가지 방식
- location.replace('url') : 로컬 state 유지 안됨 ( 리렌더 )
- router.push(url) : 로컬 state 유지 / data fetching 은 일어남
- router.push(url, as, { shallow : true }) : 로컬 state 유지 / data fetching X

<br>

#### 6.1.1 location.replace
```js
import { useRouter } from "next/router";
import { useState } from "react";

const userInfo = () => {
    const router = useRouter()

    const [clicked, setClicked] = useState(false)
    const { status = 'initial'} = router.query

    return (
        <>
            <h1>My Info</h1>
            <h1>click :  {String(clicked)}</h1>
            <h1>Status : {status} </h1>
            <button onClick={() => {
                alert('change')
                setClicked(true)
                location.replace('/username/info?status=change')
            }}>Edit : (replace)</button>
        </>
    )
}

export default userInfo;
```
![](https://velog.velcdn.com/images/hoho_0815/post/8d2a720a-2cd5-441c-9e9c-30c1c45b140d/image.gif)

- 버튼을 누르면 `true -> false` 가 되는 이유는
- __url__ 을 바꾸기 위해서 페이지를 새로 가져왔기 때문 입니다.

</br>

#### 6.1.2 router.push
```js
import { useRouter } from "next/router";
import { useState } from "react";

const userInfo = () => {
    const router = useRouter()

    const [clicked, setClicked] = useState(false)
    const { status = 'initial'} = router.query

    return (
        <>
            <h1>My Info</h1>
            <h1>click :  {String(clicked)}</h1>
            <h1>Status : {status} </h1>
            <button onClick={() => {
                alert('change')
                setClicked(true)
                router.push('/username/info?status=change')
            }}>Edit : (push)</button>
        </>
    )
}

export async function getServerSideProps(){
    console.log('server')

    return {
        props : {},
    }
}

export default userInfo;
```
- 로컬의 상태는 유지되지만, 서버에서 지속적인 data fetching 은 발생 합니다.
![](https://velog.velcdn.com/images/hoho_0815/post/9e621ee3-5623-41f3-9236-2c080f612796/image.gif)


<br>

#### 6.1.3 shallow
```js
import { useRouter } from "next/router";
import { useState } from "react";

const userInfo = () => {
    const router = useRouter()

    const [clicked, setClicked] = useState(false)
    const { status = 'initial'} = router.query

    return (
        <>
            <h1>My Info</h1>
            <h1>click :  {String(clicked)}</h1>
            <h1>Status : {status} </h1>
            <button onClick={() => {
                alert('change')
                setClicked(true)
                router.push('/username/info?status=change', undefined, {shallow : true})
            }}>Edit : (push)</button>
        </>
    )
}

export async function getServerSideProps(){
    console.log('server')

    return {
        props : {},
    }
}

export default userInfo;
```
- 로컬의 값, URL 전부 바뀌고 server 또한 불리지 않습니다.
- 정말 아무것도 안바뀌고 url 만 바뀐 상황입니다.
- 물론 동일한 페이지에서 __query__ 만 바뀌는 상황만 입니다.

***
<br>

## 정리
- Shallow Routing 은 Dynamic Routes -> slug 를 활용하는 방법
- 다중 slug -> [user]/[info].js/ , [...slug].js
- 옵셔널 slug -> [[...slug]].js
- Shallow Routing -> router.push(url, as, {shallow : true })

***
<br>

## 참고
- https://fastcampus.co.kr/dev_online_nextjs