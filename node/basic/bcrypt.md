# Bcrypt - 비밀번호 암호화

postman 을 이용하여 회원 가입을 했는데, 데이터베이스로 가면 전부 노출이 됩니다.
![](https://velog.velcdn.com/images/hoho_0815/post/ab60af8a-de9a-4515-9dba-23b81f889c17/image.png)

<br>

그리하여, 암호화를 하여 전송할 때는 위와 같이 보내도 데이터베이스에 저장할 때는 암호화 하여 관리를 해야 합니다.

***
<br>

## 설치

```js
npm install bcrypt --save
```
![](https://velog.velcdn.com/images/hoho_0815/post/de7cb500-e399-46e6-a67e-f1449f9e6d2f/image.png)


***
<br>

## 비밀번호 암호화 순서
### 1. Register Route 이동

모든 정보들을 user 라는 모델에 저장했고, `user.save(())` 를 실행하기 전에 비밀번호를 암호화 해야 합니다. ( mongoose 의 기능 사용 )

<br>

1. User.js 모델로 이동
2. 아래 코드처럼 스키마를 가져온다.

<br>

```js
// 유저 모델의 유저 정보를 저장하기 전에, 실행 한다.
userSchema.pre('save', function( next ){
    // 저장하기 전에 코드블럭에서 무엇을 실행 한 다음에
    // 다 끝나면은, 다시 아래에 보이는 user.save 코드로 들어간다.
    // next 를 사용해서 user.save((err, userInfo)) 코드로 보낸다.
    
    // 해야 할 것 : 비밀번호를 암호화 시킨다.
    next() 
}) 
```
![](https://velog.velcdn.com/images/hoho_0815/post/bc61beab-f32f-47df-96fe-67dc9548cd9e/image.png)

- 순서상 위의 빨강 상자 부분

<br>

3. https://www.npmjs.com/package/bcrypt 위의 순서 보면서 사용 ( User.js 모델 안에서 )

- 먼저 salt 를 만듭니다.
```js
const bcrypt = require('bcrypt');
// salt 를 이용해서 비밀번호를 암호화 해야 함 -> salt 를 만들 때 10자리를 만들어서 암호화 한다.
const saltRounds = 10; 
```

<br>

4. User.js - npm 에 있는 순서를 따라서 합니다 ( 설명은 주석 )
```js
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');
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

    // 비밀번호를 암호화 시킨다.
    bcrypt.genSalt(saltRounds, function(err, salt) {
        if(err) {
            return next(err)
        } else {
            // myPlaintextPassword : 암호화 된 비밀번호가 아니라 postman 에 내가 직접 입력한 비밀번호
            // const user = new User(req.body) 형식으로 req.body 를 모델에 넣었기 때문에  userSchema 의 password 를 가져오면 된다.
            bcrypt.hash(user.password, salt, function(err, hash) {
                // Store hash in your password DB.
                // hash : 암호화 된 비밀번호
                if(err) {
                    return next(err)
                } else {
                    user.password = hash
                    next() // 완성
                }
            });
        }
    });
})


const User = mongoose.model('User', userSchema)

module.exports = {User}
```

<br>

5. 비밀번호를 변경하는 경우나, 사용자 이름을 변경 또는 여러 변경 사항이 생길 때
- 뭔가를 저장해서 새로 저장할 때 마다, 아래 코드를 바꾸게 되는 것 입니다.
- 그러면 비밀번호가 아니라 이메일을 바꿔도 다시 비밀번호를 암호화 하게 됩니다.

<br>

```js
// 비밀번호를 암호화 시킨다.
    bcrypt.genSalt(saltRounds, function(err, salt) {
        if(err) {
            return next(err)
        } else {
            // myPlaintextPassword : 암호화 된 비밀번호가 아니라 postman 에 내가 직접 입력한 비밀번호
            // const user = new User(req.body) 형식으로 req.body 를 모델에 넣었기 때문에  userSchema 의 password 를 가져오면 된다.
            bcrypt.hash(user.password, salt, function(err, hash) {
                // Store hash in your password DB.
                // hash : 암호화 된 비밀번호
                if(err) {
                    return next(err)
                } else {
                    user.password = hash
                    next() // 완성
                }
            });
        }
    });
```

<br>

- __비밀번호 암호화는 비밀번호를 바꾸는 경우에만 시행이 되야 합니다.__
- `isModified` 를 사용하여 조건을 사용 합니다.

<br>

```js
userSchema.pre('save', function(next){
    var user = this; // user 가 위의 객체들을 가리킨다.

    // 모델안의 필드 중 password 가 변화 될 때만 아래 코드가 실행된다.
    if(user.isModified('password')){
        // 비밀번호를 암호화 시킨다.
        bcrypt.genSalt(saltRounds, function(err, salt) {
            if(err) {
                return next(err)
            } else {
                // myPlaintextPassword : 암호화 된 비밀번호가 아니라 postman 에 내가 직접 입력한 비밀번호
                // const user = new User(req.body) 형식으로 req.body 를 모델에 넣었기 때문에  userSchema 의 password 를 가져오면 된다.
                bcrypt.hash(user.password, salt, function(err, hash) {
                    // Store hash in your password DB.
                    // hash : 암호화 된 비밀번호
                    if(err) {
                        return next(err)
                    } else {
                        user.password = hash
                        next() // 완성
                    }
                });
            }
        });
    }    
})
```

***
<br>

## 테스트
먼저 서버를 열고 아래 처럼 post-send 를 하였습니다.
![](https://velog.velcdn.com/images/hoho_0815/post/8d079ded-44cb-44b9-8662-1c729b46758c/image.png)

<br>

그리고, MongoDB -> cluster -> connection 에서 DB 에 들어있는 것을 확인하면
![](https://velog.velcdn.com/images/hoho_0815/post/9f8467d0-7ad9-4055-b010-de855e0affef/image.png)
암호화가 정상적으로 된 것을 확인 할 수 있습니다.

***
<br>

## 참고
- 따라하며 배우는 노드, 리액트 