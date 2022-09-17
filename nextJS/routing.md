# Next.js - Routing
Next.js 의 Router 는 file-system 기반
- file-system : 파일을 만들면 그 즉시 라우터로 인지되고 주소가 맵핑이 됩니다.
- pages/ 
- src/pages/

> 리액트에서는 별도의 Router 를 제공하지 않기 때문에 별도의 라이브러리를 사용해야 하지만, Next.js 는 기본적으로 제공을 합니다.

***
<br>

## pages/ OR src/pages 둘 중 우선순위는?
- 우선순위는 __pages/__ 가 우선 입니다.

### pages/index.js
```js
import Link from 'next/link'
import { useEffect, useState } from 'react'

export async function getStaticProps(){
  console.log('server')

  return {
    props : {time : new Date().toISOString()}
  }
}

export default function Home() {

  return (
    <div>
      <h1><Link href="/ssg">SSG로 - pages -</Link></h1>
      <h1><Link href="/isr">ISR로 - pages -</Link></h1>
    </div>
  )
}
```

<br>

### src/pages/index.js
```js
import Link from 'next/link'
import { useEffect, useState } from 'react'

export async function getStaticProps(){
  console.log('server')

  return {
    props : {time : new Date().toISOString()}
  }
}

export default function Home() {

  return (
    <div>
      <h1><Link href="/ssg">SSG로</Link></h1>
      <h1><Link href="/isr">ISR로</Link></h1>
    </div>
  )
}
```
![](https://velog.velcdn.com/images/hoho_0815/post/19dcb2c1-6696-475e-9f9e-2398f5fafb86/image.png)
- 화면에는 __pages__ 의 index.js 가 나옵니다.

> 즉, pages 폴더가 있다면, src 폴더 안의 pages 는 무시가 됩니다.

***
<br>

## Nested routes
- pages/product/first-item.js => /product/first-item
- pages/settings/my/info.js => /settings/my/info

![](https://velog.velcdn.com/images/hoho_0815/post/d4353167-4630-4d24-9ee4-91f4c208806c/image.png)
![](https://velog.velcdn.com/images/hoho_0815/post/5f371537-68da-4f9e-9f1c-4b090dfcba51/image.png)

<br>

### Tip, next.js 는 절대 경로 접근이 가능 합니다.
- 파일 깊이가 깊어질수록 ../ 을 하는 일이 많아집니다.
- 하지만 아래 설정을 하면 손쉽게 절대 경로로 접근이 가능 합니다.

#### jsconfig.json
```js
{
	"compilerOptions" : {
        "baseUrl" : "src"
    }
}
```

#### first-item.js
```js
// import Hello from '../../src/components/Hello'; // 기존
import Hello from '/src/components/Hello';

const first_item = () => {
    return (
        <>
            <h1>first_item</h1>
            <Hello/>
        </>
    )
}

export default first_item;
```

- 위의 이미지 처럼 동일한 결과가 나옵니다.

***
<br>

## Nested routes 주의할점
/product/first-item 까지는 접근이 가능하지만, product 로는 접근이 안될까?

> 정답은 그냥 /product 를 입력하면 404 가 나옵니다.
![](https://velog.velcdn.com/images/hoho_0815/post/161699af-135e-4f86-bae1-d94e19485ee9/image.png)


- product 에 바로 접근하고 싶으면 해당 __index.js__ 가 있어야 합니다.
![](https://velog.velcdn.com/images/hoho_0815/post/25be509b-fada-4fbb-9ffc-36ba0b373455/image.png)

![](https://velog.velcdn.com/images/hoho_0815/post/6c29d609-111f-46ca-83c0-b5b2537e61a2/image.png)

***
<br>

## slug
- pages/category/[slug].js => /category/:slug (ex. /category/food )
- pages/[username]/info.js => /:username/info (ex. /hyunho/info )

> 대괄호를 하고 어떤 값을 넣으면 그 값의 와일드카드처럼 동작을 합니다.

![](https://velog.velcdn.com/images/hoho_0815/post/0197ef63-e44d-4c3a-bfb8-c4f9b7d00d1c/image.gif)


![](https://velog.velcdn.com/images/hoho_0815/post/a9dbe425-fb93-47aa-8799-0de2d5dfa681/image.png)

- 지금은 slug 를 한 뎁스만 사용했지만 __[...slug].js__ 하면 
-> (ex. /cart/2022/06/24) 무한하게 접근 가능 합니다.


***
<br>

## 정리
- Router -> File system 을 토대로 구현
- pages/ 혹은 src/pages -> pages/ 가 우선순위, 하나만 가능
- 프로젝트 설정 -> jsconfig.json(절대경로)
- Slug -> 다양한 위계의 Dynamic 제공

***
<br>

## 참고
- https://fastcampus.co.kr/dev_online_nextjs
