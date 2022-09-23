# MongoDB 연결
[https://www.mongodb.com/](https://www.mongodb.com/)

<br>

## 클러스터 ?
보통 데이터베이스 구축의 경우 1개의 서버로 하나의 데이터베이스를 구축해서 사용하는 편입니다. <br>

하지만 1개의 서버가 하나의 데이터베이스를 사용할 경우 이 서버가 죽으면 서비스가 죽는 현상이 발생 합니다. <br>

또한 사용자가 엄청나게 많이 유입이 되었을 때, 서버가 견디지 못하고 뻗을 수 있습니다.

<br>

> 이 처럼 하나의 데이터베이스를 여러 개의 서버로 구축되는 경우를 클러스터(Cluster) 라고 합니다.

<br>

## 클러스터 만들기

![](https://velog.velcdn.com/images/hoho_0815/post/177d23b1-9d03-4731-a8c6-7927df87c9ab/image.png)



1. 오른쪽 상단에 Try Free 클릭 ( 회원가입이 되어있지 않다면 )
2. sign in 눌러서 로그인
3. All Clusters 클릭 만약 처음이라서 아래와 같은 화면이 보이면 __Build a Database__ 클릭


![](https://velog.velcdn.com/images/hoho_0815/post/47a49fd7-556d-4abf-95d4-20bc8cac3c87/image.png)

4. 그 후 free 라고 있는 shared 클릭

![](https://velog.velcdn.com/images/hoho_0815/post/2cd4eb4e-b688-4f36-a177-08afc052c062/image.png)

5. 위와 같은 화면이 나온다.
6. aws(이건 자유) - 나라 선택 
7. Cluster Tier 에서 MO Sandbox 클릭
8. Cluster Name 은 자신이 원하는 걸로
9. 그 후 create cluster 클릭

![](https://velog.velcdn.com/images/hoho_0815/post/2ee828e2-cb90-4262-b38a-153020cf5044/image.png)

10. 연결 인증을 위한 이름과 비밀번호 사용자 생성

![](https://velog.velcdn.com/images/hoho_0815/post/bc4452aa-ea17-4bd1-8876-bc0499348cae/image.png)

11. 내 현재 IP 주소 추가 클릭
12. 완료 및 닫기
13. 그 다음 메인-Atlas-Connect 클릭

![](https://velog.velcdn.com/images/hoho_0815/post/002b3a9c-aa2e-49e7-bee1-fd775b604873/image.png)

14. `Connect Your Application` 선택

![](https://velog.velcdn.com/images/hoho_0815/post/36ef684c-31b3-4a8b-86d0-863843b12fd9/image.png)

15. 위에 보이는 코드 복사해서 가지고 있기

<br>

## Mongoose 
MongoDB 를 편하게 사용할 수 있게 해주는 Object Modeling Tool 입니다.

```js
npm install mongoose --save
```

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/34ceaa62-0335-415f-83d0-869c5a131415/image.png)


- package.json 에 추가된걸 확인 할 수 있습니다.

<br>

### Mongoose 모듈 가져오기
#### index.js
```js
const express = require('express') // express 모듈을 가져온다.
const app = express() 
const port = 3000 

const mongoose = require('mongoose')
mongoose.connect('mongodb+srv://choihyunho:<password>@boilerplate.feh9f3z.mongodb.net/?retryWrites=true&w=majority',{
    useNewUrlParser : true, useUnifiedTopology : true, useCreateIndex : true, useFindAndModify : false
})


app.get('/', (req, res) => res.send('Hello World!')) // 루트 디렉토리에 hello world! 출력

app.listen(port, () => console.log(`Example app listening on port ${port}!`)) // 위의 포트(5000) 에서 실행
```
- 위와 같은 형식으로 Mongoose 모듈을 추가 합니다.
- use와 비슷한 코드는 에러를 방지하기 위한 코드 입니다.
- 그 다음 `<password>` 부분에 사용자의 비밀번호를 입력 합니다(위에서 만든)

```js
mongoose.connect('mongodb+srv://choihyunho:abcd1234@boilerplate.feh9f3z.mongodb.net/?retryWrites=true&w=majority',{
    useNewUrlParser : true, useUnifiedTopology : true, useCreateIndex : true, useFindAndModify : false
})
```

<br>

## MongoDB 연결 최종 코드
```js
const express = require('express') // express 모듈을 가져온다.
const app = express() 
const port = 3000 

const mongoose = require('mongoose')
mongoose.connect('mongodb+srv://choihyunho:abcd1234@boilerplate.feh9f3z.mongodb.net/?retryWrites=true&w=majority',{
    useNewUrlParser : true, useUnifiedTopology : true
}).then(()=> console.log('MongoDB Connected...')).catch(err => console.log('error'))


app.get('/', (req, res) => res.send('Hello World!')) // 루트 디렉토리에 hello world! 출력

app.listen(port, () => console.log(`Example app listening on port ${port}!`)) // 위의 포트(5000) 에서 실행
```

![](https://velog.velcdn.com/images/hoho_0815/post/3b51e942-ab29-4fae-bbbd-a66a5ecfa92b/image.png)

- 연결이 올바르게 되었습니다.

***
<br>

## 참고
- 따라하면서 배우는 노드 리액트 기본