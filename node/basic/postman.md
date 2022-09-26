# BodyParser & PostMan & 회원 가입 기능
## Client-Server 통신하는 법
client 는 예시로 브라우저(chrome) 이라고 생각 하면 됩니다.<br>
만약, github에 로그인을 하려고 할 때, client 를 이용해서 이름, 이메일, 패스워드 등을 사용하여 서버에 정보를 전송해야 합니다.

***
<br>

## BodyParser
클라이언트에서 정보를 입력하여 전송하면 서버에서 정보를 받아야 하는데 그 때 사용하는 dependency 입니다.<br>
Body 데이터를 분석(parser) 해서 req.body 로 출력 해주는 것

```js
npm install body-parser --save
```
![](https://velog.velcdn.com/images/hoho_0815/post/db2b3bec-9222-4f88-9310-d00f0b0a04c8/image.png)

<br>

### index.js 에 BodyParser 코드 추가

```js
const bodyParser = require('body-parser')

// application/x-www-form-urlencoded 와 같은 형식의 데이터를 분석 할 수 있게 해준다.
app.use(bodyParser.urlencoded({extended : true}));

// application/json 형식의 데이터를 분석하여 가져온다.
app.use(bodyParser.json());
```


***
<br>

## PostMan
Postman은 개발한 API를 테스트하고, 테스트 결과를 공유하여 API 개발의 생산성을 높여주는 플랫폼 입니다.<br>
아직 정보를 입력할 화면을 만들지 않았기에, 데이터 전송이 잘되는지 PostMan 을 이용하여 테스트 할 수 있습니다.
<br>

- [PostMan Download](https://www.postman.com/downloads/)

***
<br>

## Register Route 만들기
### index.js

```js
// 백엔드 서버 시작점

const express = require('express') // express 모듈을 가져온다.
const app = express() 
const port = 3000 
const bodyParser = require('body-parser')

// application/x-www-form-urlencoded 와 같은 형식의 데이터를 분석 할 수 있게 해준다.
app.use(bodyParser.urlencoded({extended : true}));

// application/json 형식의 데이터를 분석하여 가져온다.
app.use(bodyParser.json());


const mongoose = require('mongoose')
mongoose.connect('mongodb+srv://choihyunho:gusgh0816@boilerplate.feh9f3z.mongodb.net/?retryWrites=true&w=majority',{
    useNewUrlParser : true, useUnifiedTopology : true
}).then(()=> console.log('MongoDB Connected...')).catch(err => console.log('error'))

const { User } = require('./models/User');


app.get('/', (req, res) => res.send('Hello World!')) // 루트 디렉토리에 hello world! 출력

// 회원가입을 위한 route
app.post('/register', (req, res) => {
    // 회원 가입 할 때 필요한 정보들을 client 에서 가져오면
    // 그것들을 데이터베이스에 넣어준다. -> User.js 모델을 가져온다.

    const user = new User(req.body) // json 형식으로 정보가 들어온다.

    // MongoDB 메서드 -> 정보들이 user 모델에 저장이 된다.
    user.save((err, userInfo)=>{
        if(err){
            return res.json({success : false, err})
        } else {
            return res.status(200).json({success : true})
        }
    }) 
})

app.listen(port, () => console.log(`Example app listening on port ${port}!`)) // 위의 포트(5000) 에서 실행
```

***
<br>

## PostMan 에서 테스트
### 1. app 가동
- npm run start

<br>

#### 혹시 MongoDB 가 연결되지 않으면?
![](https://velog.velcdn.com/images/hoho_0815/post/cbce8a29-0440-46c1-9c52-1af4afc6fda4/image.png)

- 해결한 에러는 처음에 첫 번째 개인 집에서 공부할 때 쓰던 ip만 연결하고 있다가 장소가 달라지니까 동시에 ip가 달라짐으로, 자연스레 연결이 되지 않았습니다.
- 그리하여, ADD IP ADDRESS 를 클릭하여 어디서든 접속 가능한 ip 를 하나 추가 하여 해결 했습니다.

<br>

### 2. PostMan 사용
1. Collections 의 + 를 눌러서

![](https://velog.velcdn.com/images/hoho_0815/post/0ab204ec-8ac5-4052-a957-d5dd8147db51/image.png)

<br>

2. 현재 사용할 post 를 선택

![](https://velog.velcdn.com/images/hoho_0815/post/f1d06e4d-0b12-483e-8946-a311eb756970/image.png)

3. url 입력하고 body -> raw 차례대로 선택
```js
http://localhost:3000/register
```
<br>

- 아래의 정보를 url 에 가져왔습니다.

<br>

```js
const port = 3000 

// 회원가입을 위한 route
app.post('/register', (req, res) => {
    // 회원 가입 할 때 필요한 정보들을 client 에서 가져오면
    // 그것들을 데이터베이스에 넣어준다. -> User.js 모델을 가져온다.

    const user = new User(req.body) // json 형식으로 정보가 들어온다.

    // MongoDB 메서드 -> 정보들이 user 모델에 저장이 된다.
    user.save((err, userInfo)=>{
        if(err){
            return res.json({success : false, err})
        } else {
            return res.status(200).json({success : true})
        }
    }) 
})
```

![](https://velog.velcdn.com/images/hoho_0815/post/209efc91-654e-44e6-8c97-53dba50ea1cc/image.png)

<br>

3. json 형식으로 보낼 예정
- 현재 입력할 화면이 따로 없으므로 PostMan 안에서 직접 입력 합니다.
- 입력할 때 기존에 제작했던 `User.js` 모델 참고

```js
name : {
        type : String,
        maxlenght : 50
    },
    email : {
        type : String,
        trim : true, // hyun ho@naver.com 이라고 하면, 이름 중간에 공백이 있는데 이것을 없애주는 역할을 한다.
        unique : 1,  // 같은 이메일은 사용 할 수 없게 
    },
    password : {
        type : String,
        minlenght : 5
    },
    lastname : {
        type : String,
        maxlenght : 50
    },
    role : {
        // 어떤 유저가 관리자가 될 수도, 일반 유저가 될 수도 해당 등급에 대한 조건
        type : Number,
        default : 0
    },
    image: String,
    token : {
        // 유효성 
        type : String
    },
    tokenExp : {
        // 토큰의 유효기간
        type : Number
    }
```

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/b25cf37b-a531-4b3c-9331-1d56bf406bbb/image.png)

- 아래 `{}` 안에 name, email, password 를 `User.js` 모델에서 정의한대로 작성 하였습니다.
- 그 후 JSON 형식으로 바꾸고, Send 를 누르면 아래와 같은 `success` 결과가 나옵니다.
```js
{
    "success" : true 
}
```
- 위와 같은 메시지가 나오는 이유는 index.js 에 아래와 같이 데이터를 전송하거나 실패할 때 코드를 작성했기 때문 입니다.
```js
 user.save((err, userInfo)=>{
        if(err){
            return res.json({success : false, err})
        } else {
            return res.status(200).json({success : true})
        }
    }) 
```

<br>

### 주의 !
```js
type : String
```

- String 타입을 큰 따옴표가 아닌 작은 따옴표로 묶으면 에러가 뜹니다.

![](https://velog.velcdn.com/images/hoho_0815/post/2b39f67d-c700-459f-aefd-f9a40396a2e6/image.png)

***
<br>

## 참고
- 따라하면서 배우는 노드, 리액트 기초