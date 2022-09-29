# Auth 기능
어떤 사이트를 들어갔을 때, 여러가지 페이지들이 있다. <br>
어떤 곳은 로그인이 된 유저만, 다른 페이지는 로그인 안해도 누구나 이용 가능한, 또는 그 사이트의 관리자만 이용하는 곳
- 이런 것을 체크하기 위해서 만듭니다.

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/43b57203-0d34-41e7-b479-57875df59e09/image.png)

<br>

1. 저번에 토큰을 만들어서, 유저 정보에 넣어 줬습니다.
2. 토큰 관리는 클라이언트는 쿠키에서, 서버는 데이터베이스에서 하고
3. 위의 두가지를 이용해서 두가지 토큰이 동일한지 지속적으로 체크하는 것 입니다.

<br>

## 순서
### 1. cookie 에 저장된 토큰을 서버에서 가져와서 복호화(Decode) 를 합니다.
#### index.js 에서 auth route 만들기

```js
// auth 기능
// 가운데 auth 는 미들웨어 : 중간에서 처리
app.get('/api/user/auth', auth ,(req, res) => {
    // 여기까지 미들웨어를 통과해 왔다는 뜻 -> 인증이 성공적으로 진행 됬다는 뜻.
    res.status(200).json({
        // 제공할 정보 - User.js
        // 가능한 이유는 auth.js 에서 req.user = userInfo; 정보를 가져왔기 때문에
        _id : req.user._id, 
        isAdmin : req.user.role === 0 ? false : true,
        isAuth : true,
        email : req.user.email,
        name : req.user.name,
        lastname : req.user.lastname,
        role : req.user.role,
        image : req.user.image
    })
})
```

위에 보이는 auth 의 위치 (middle/auth.js)
```js
let auth = (req, res, next) => {
    // 인증 처리를 하는 곳
}

module.exports = {auth};
```

<br>

### 2. 메서드 생성 (User 모델)

```js
userSchema.statics.findByToken = function(token, cb){
    var user = this;

    // 토큰을 복호화 한다. (token, user._id + 'string' -> secretToken)
    jwt.verify(token, 'secretToken', function(err, decoded){
        // 유저 아이디를 이용해서 유저를 찾은 다음
        // 클라이언트에서 가져온 토큰과 데이터베이스에 보관된 토큰이 일치하는지 확인
        user.findOne({"_id" : decoded, "token" : token}, function(err, user){
            if(err) return cb(err);
            cb(null, user);
        })
    })
}
```

<br>

### 3. auth.js 
```js
const { User } = require("../models/User");

// 미들웨어에서 처리하는 곳
let auth = (req, res, next) => {
    // 인증 처리를 하는 곳
    // 1. 클라이언트 쿠키에서 토큰을 가져오는 코드
    let token = req.cookies.x_auth; 

    // 2. 가져온 토큰을 복호화 한 후 유저를 찾는 코드,
    User.findByToken(token, (err, userInfo) => {
        if(err) { throw err; }
        if(!userInfo){
            return res.json({isAuth : false, error : true})
        }

        req.token = token;
        req.user = userInfo;
        next();
    })
}

module.exports = {auth};
```

***
<br>

## 참고
- 따라하며 배우는 노드, 리액트