# 비밀 설정 정보 관리 - 환경 변수

소스안에 있는 비밀 정보를 보호하는 것 <br>
현재 소스에서 비밀 정보라 함은, 

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/3d45a00a-ddd9-4dfc-9041-fd290d70234e/image.png)

<br>

애플리케이션에 연결을 할 때 사용하는 MongoDB 아이디와 비밀번호 입니다. <br>
만약, github 에다 소스를 올리면 위의 정보까지 다 보이니까 다른 사람이 위의 정보를 사용해서 MongoDB 를 사용 할 수 있다.

***
<br>

## config 폴더 생성
개발은 두 가지 환경에서 진행 할 수 있습니다. 

![](https://velog.velcdn.com/images/hoho_0815/post/9cbeb0bb-efd6-4e52-9910-93940deb1f8a/image.png)

<br>

1. local 개발 모드 ( dev.js ) <br>
2. production ( deploy 배포 이후, prod.js )

<br>

### 폴더 구조

![](https://velog.velcdn.com/images/hoho_0815/post/37815bc7-dec2-4c09-a00a-153c827f3feb/image.png)

<br>

#### dev.js
```js
module.exports = {
    mongoURI : 'mongodb+srv://choihyunho:gusgh0816@boilerplate.feh9f3z.mongodb.net/?retryWrites=true&w=majority'
}
```

<br>

#### prod.js
```js
module.exports = {
    mongoURI : process.env.MONGO_URI
}
```
- 위의 __MONGO_URI__ 는 Heroku 형식을 따른 것 입니다.

<br>

#### key.js
```js
if(process.env.NODE_ENV === 'production'){
    module.exports = require('./prod');
} else {
    module.exports = require('./dev');
}
```

<br>

### index.js 수정
```js
const config = require('./config/key') // 추가하고

const mongoose = require('mongoose') // connect 부분 수정
mongoose.connect(config.mongoURI ,{
    useNewUrlParser : true, useUnifiedTopology : true
}).then(()=> console.log('MongoDB Connected...')).catch(err => console.log('error'))
```

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/450c0bc7-6316-4c0b-aacb-2c7bceec5fd2/image.png)

정상적으로 서버와 연결이 되었습니다.

***
<br>

## dev.js 는 현재 github 에서 보이면 안된다.
__.gitignore__ 에 dev.js 추가 

***
<br>

## 참고
- 따라하며 배우는 노드, 리액트