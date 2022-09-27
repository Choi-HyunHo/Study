# Bcrypt - 로그인 기능

## Login Route 만들기
### User.js (모델) 
#### 비밀번호 비교 하는 메서드 생성
```js
userSchema.methods.comparePassword = function(plainPassword, cd){
    // planPassword : 1234 등, 데이터베이스에 있는 암호화 된 비밀번호 "$2b$10$rwffndFzLFvUpxh6P1ql7u4bWg/4OXbdJwKYFUBSZe4sJeoIgXC5C"
    // 1234 도 암호화화해서 디비와 같은지 확인(오른쪽 문자)
    bcrypt.compare(plainPassword, this.password, function(err, isMatch){
        if(err){
            return cd(err)
        } else {
            cb(null, isMatch) // 에러는 없고, 비밀번호는 같다 
        }
    })
}
```

<br>

- comparePassword 는 사용자 입맛에 맞게 이름을 정하면 됩니다.
- `bcrypt` 라이브러리를 사용하여 생성
- plainPassword 는 사용자가 입력하는 비밀번호
- this.password 는 스키마에 정의되어 있는 password 객체
- function(err, isMatch) 는 입력에 따라 입력 받는 콜백 함수.
- 위의 코드를 index.js 에서 2번 부분으로 가져옵니다.

***
<br>

## 비밀번호가 맞다면 토큰 생성하는 메서드 생성
### JSONWEBTOKEN 라이브러리
토큰 생성을 위해 다운로드 받습니다.

```js
npm install jsonwebtoken --save
```

![](https://velog.velcdn.com/images/hoho_0815/post/c6e3da12-6c79-4ccd-967d-67be6a435c0c/image.png)


<br>

### 과정
```js
1. jwt.sign(user._id, , 'secretToken') 
2. user._id + 'secretToken' = token 
```

- user의 id 는 데이터베이스 _id, 'secretToken' 자리는 개발자 마음대로

![](https://velog.velcdn.com/images/hoho_0815/post/b46f36e7-cb1b-4905-9715-196c0954f0f7/image.png)

- user._id + 'secretToken' = token , 두 개의 요소를 합쳐 토큰을 만듭니다.
- 토큰을 해석을 할 때, __`secretToken`__ 을 넣으면, __'user._id'__ 가 나옵니다.
- 토큰을 가지고 사용자가 누구인지 알 수 있습니다.

<br>

### 저장하는 위치는 쿠기
```js
npm install cookie-parser --save
```

#### index.js 에 아래 코드 추가
```js
const cookieParser = require('cookie-parser')
app.use(cookieParser)
```

***
<br>

## 전체 코드
### index.js
```js
// 백엔드 서버 시작점

const express = require('express'); // express 모듈을 가져온다.
const app = express();
const port = 3000 
const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');

const config = require('./config/key');

// application/x-www-form-urlencoded 와 같은 형식의 데이터를 분석 할 수 있게 해준다.
app.use(bodyParser.urlencoded({extended : true}));

// application/json 형식의 데이터를 분석하여 가져온다.
app.use(bodyParser.json());
app.use(cookieParser());


const mongoose = require('mongoose');
mongoose.connect(config.mongoURI ,{
    useNewUrlParser : true, useUnifiedTopology : true
}).then(()=> console.log('MongoDB Connected...')).catch(err => console.log('error'));

const { User } = require('./models/User');


app.get('/', (req, res) => res.send('Hello World! 123')); // 루트 디렉토리에 hello world! 출력

// 회원가입을 위한 route
app.post('/register', (req, res) => {
    // 회원 가입 할 때 필요한 정보들을 client 에서 가져오면
    // 그것들을 데이터베이스에 넣어준다. -> User.js 모델을 가져온다.

    const user = new User(req.body) // json 형식으로 정보가 들어온다.

    // MongoDB 메서드 -> 정보들이 user 모델에 저장이 된다.
    user.save((err, userInfo)=>{
        if(err){
            return res.json({success : false, err});
        } else {
            return res.status(200).json({success : true});
        }
    }) 
})

app.post('/login', (req, res) => {
    // 1. 요청된 이메일을 데이터베이스에서 있는지 찾는다.
    User.findOne({email : req.body.email}, (err, userInfo) => {
        if(!userInfo){
            return res.json({
                loginSuccess : false,
                message : '제공된 이메일에 해당하는 유저가 없습니다.'
            })
        }
        // 2. 요청한 이메일이 데이터베이스에 있다면, 비밀번호가 같은지 확인
        userInfo.comparePassword(req.body.password, (err, isMatch) => {
            if(!isMatch){
                // 비밀번호가 같지 않다.
                return res.json({loginSuccess : false, message : '비밀번호가 틀렸습니다.'});
            } else {
                // 3. 비밀번호 까지 같다면 유저를 위한 token 생성
                userInfo.getToken((err, user) => {
                    if(err){
                        return res.status(400).send(err);
                    } else {
                        // 토큰을 저장한다. 쿠키 ( 다른 방법도 있다. )
                        // 쿠키 저장하려면 라이브러리 다운 - express 에서 제공하는 ( npm install cookie-parser --save)
                        res.cookie("x-auth", user.token)
                            .status(200) // 성공
                            .json({loginSuccess : true, userId : user._id});
                    }
                })
            }
        })
    })
})

app.listen(port, () => console.log(`Example app listening on port ${port}!`)) // 위의 포트(5000) 에서 실행

```

<br>

### User.js
```js
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken')
const saltRounds = 10; // salt 를 이용해서 비밀번호를 암호화 해야 함 -> salt 를 만들 때 10자리를 만들어서 암호화 한다.

const userSchema = mongoose.Schema({
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
})

userSchema.pre('save', function(next){
    var user = this; // user 가 위의 객체들을 가리킨다.

    // 모델안의 필드 중 password 가 변화 될 때만 아래 코드가 실행된다.
    if(user.isModified('password')){
        // 비밀번호를 암호화 시킨다.
        bcrypt.genSalt(saltRounds, function(err, salt) {
            if(err) {
                return next(err);
            } else {
                // myPlaintextPassword : 암호화 된 비밀번호가 아니라 postman 에 내가 직접 입력한 비밀번호
                // const user = new User(req.body) 형식으로 req.body 를 모델에 넣었기 때문에  userSchema 의 password 를 가져오면 된다.
                bcrypt.hash(user.password, salt, function(err, hash) {
                    // Store hash in your password DB.
                    // hash : 암호화 된 비밀번호
                    if(err) {
                        return next(err);
                    } else {
                        user.password = hash
                        next() // 완성
                    }
                });
            } 
        });
    } else {
        next();
    }    
})

userSchema.methods.comparePassword = function(plainPassword, cd){
    // planPassword : 1234 등, 데이터베이스에 있는 암호화 된 비밀번호 "$2b$10$rwffndFzLFvUpxh6P1ql7u4bWg/4OXbdJwKYFUBSZe4sJeoIgXC5C"
    // 1234 도 암호화화해서 디비와 같은지 확인(오른쪽 문자)
    bcrypt.compare(plainPassword, this.password, function(err, isMatch){
        if(err){
            return cd(err);
        } else {
            return cd(null, isMatch); // 에러는 없고, 비밀번호는 같다 
        }
    })
}

userSchema.methods.getToken = function(cb){
    // jsonwebtoken 을 이용해서 토큰 생성
    let user = this;
    
    let token = jwt.sign(user._id.toHexString(), 'secretToken')
    user.token = token
    user.save(function(err, user){
        if(err){
            return cb(err);
        } else {
            cb(null, user) // 에러는 없고 유저정보 전달 -> index.js
        }
    })
}

const User = mongoose.model('User', userSchema)

module.exports = {User}
```

***
<br>

## PostMan 을 통해 테스트

1. __+__ 을 눌러서 post 생성
![](https://velog.velcdn.com/images/hoho_0815/post/ebd6cd10-8277-42ab-895c-e5129e711137/image.png)

<br>

2. 서버 기동 후, body -> raw -> json 누르고 화면에 입력 <br>

2.1 없는 정보를 입력 했을 때
![](https://velog.velcdn.com/images/hoho_0815/post/c3282104-e4fd-4f14-b1a6-c5fcb3c1b356/image.png)

<br>

2.2  데이터베이스에 정보가 있을 때
![](https://velog.velcdn.com/images/hoho_0815/post/ba55bd22-72ff-45f0-9ae5-45fa266ac6c7/image.png)


***
<br>

## 참고
- 따라하며 배우는 노드, 리액트 