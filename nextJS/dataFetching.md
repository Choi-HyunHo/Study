# Next.js - Data Fetching
데이터 가져오기 
-> 화면에 무엇인가를 그리려면 어디선가 __데이터를 가져와야__ 합니다.

***
<br>

## 1. SSR (Server Side Render)
- __서버__ 에서부터 
- 그린다 : 데이터를 가져와서 나타낸다.

> 즉, 서버가 데이터를 가져와서 그린다.

<br>

### 1.2 SSR 을 담당하는 함수
#### getServerSideProps

<br>

#### index.js
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
- SSR 과 정반대로 __클라이언트__가 그립니다.
> 즉, 클라이언트가 데이터를 가져와서 그립니다.

<br>

### 2.1 CSR 을 담당하는 함수
- 따로 존재하지 않습니다.
- 기존 React 사용법과 동일 합니다.
- next.js 의 Link 등...

***
<br>

## 3. SSG ( Static-Site Generation )
- __정적인 사이트__ 를 생성합니다.

<br>

### 3.1 SSG 를 담당하는 함수
#### getStaticProps
- yarn dev 를 사용하는 개발환경에서는 제대로 동작하지 않습니다. (SSG 가 SSR 처럼 동작 합니다!)
- 정확하게 확인하려면 빌드를 한 후 yarn start 를 해야 합니다.
- __SSG__ 에서는 새로고침을 해도 페이지가 변함없이 동일 합니다. ( 콘솔조차 찍히지 않습니다. )

> yarn build 를 할 때 페이지를 미리 만들었다는 뜻 입니다.
즉, 빌드하는 타이밍에 데이터를 다 가져와서 보여줄 페이지를 static 한 페이지를 미리 생성한 것입니다.

<br>

#### ssg.js
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
- 위의 예시처럼 dev 환경에서는 새로고침하면 시간이 계속 변하지만
(SSR 처럼 node에 로그가 찍힙니다.)
- 빌드하고 배포한 후 실행하면 정적인 페이지가 만들어집니다. 
(새로고침해도 시간이 화면의 시간이 변하지 않습니다.)
![](https://velog.velcdn.com/images/hoho_0815/post/36f3492b-43f2-4ca4-9781-66e34c3cffe4/image.gif)
<br>

### 3.2 동적이지도 않고 정적인데 필요한 이유는 ?
- SSR 은 항상 접근 할 때 마다, 즉 새로고침 할 때 마다 서버가 동작 합니다

> 즉, 데이터를 그릴 때 마다 서버가 동작하고 
여러 사용자가 접근한다고 하면 서버의 부하가 많아집니다.


<br>

- 하지만 __SSG__ 를 사용하면

> 정적인 페이지 한정이지만, 그 페이지는 누가 접근해도 빌드할 때 이미 만들어져있는 것을 보여주기 때문에 서버의 부하 없이 서비스를 제공 할 수 있습니다.

***
<br>

## 4. ISR ( Incremental Static Regeneration )

- __증분 정적 사이트__를 재생성 합니다.
- 재생성한다 : (특정 주기로) 데이터를 가져와서 다시 그립니다.

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
