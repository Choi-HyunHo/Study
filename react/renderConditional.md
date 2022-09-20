# 조건부 렌더링 ( Conditional Rendering )
__Condition__ 이라고 하면 __조건, 상태__ 라는 뜻 입니다. <br>
평소 '오늘 컨디션이 좋다/ 나쁘다' 라고 표현할 때 컨디션은 __상태__ 라는 의미를 가지고 있습니다. <br>
하지만 컴퓨터 프로그래밍에서의 컨디션은 __조건__ 을 의미 합니다.

> 즉, 공식 영문 용어인 __Conditional Rendering__ 을 __조건에 따른 렌더링__ 이라고 해석하고 줄여서 <br> __조건부 렌더링__ 이라고 부릅니다.

<br>

- 결과는 true 아니면 false 가 나옵니다.

***
<br>

## 예시
```js
const UserGreeting = (props) => {
    return <h1>다시 오셨군요!</h1>;
}

const GuestGreeting = (props) => {
    return <h1>회원가입을 해주세요.</h1>;
}
```

- UserGreeting 컴포넌트는 이미 회원인 사용자에게 보여줄 메시지를 출력하는 컴포넌트
- GuestGreeting 컴포넌트는 아직 가입하지 않은 게스트 사용자에게 보여줄 메시지 출력하는 컴포넌트
- 회원인지 아닌지에 따라 이 두개의 컴포넌트를 선택적으로 보여줘야 합니다.

<br>

### 조건부 렌더링 적용
```js
const Greeting = (props) =>{
    const isLoggedIn = props.isLoggedIn;
    
    if(isLoggedIn){
        <UserGreeting/>;
    } else {
        <GuestGreeting/>;
    }
}
```

- Greeting 컴포넌트는 isLoggedIn 이라는 변수의 값이 __true__ 일 때, `<UserGreeting/>` 출력하고
- isLoggedIn 값이 __false__ 이면, `<GuestGreeting/>` 컴포넌트를 출력 합니다.

***
<br>

## 인라인 조건 ( if )
코드를 별도로 분리된 곳에 작성하지 않고 해당 코드가 필요한 곳에 직접 집어넣는다는 뜻 입니다.<br>

> 즉, 인라인 조건 이라고 하면 __조건문을 코드 안에 집어넣는 것__ 이라는 뜻을 가지고 있습니다.

<br>

### && 와 || 사용
조건문을 코드 안에 집어 넣는다고 해서 if 나 if-else 를 넣는 것은 아니고 동일한 효과를 낼 수 있는 `&&` 과 `||` 을 사용 합니다.

<br>

- && 연산자는 __AND 연산__ 이라고 부르는데 __양쪽에 나오는 조건문이 모두 true인 경우에만 전체 결과가 true__
- 즉, 첫 번째 조건문이 true 이면 두 번째 조건문을 평가하고, 첫 번째 조건문이 false 이면 전체 결과는 false 가 되므로, 평가하지 않습니다.

<br>

> 프로그래밍에서 이것을 __단축 평가__ 라고 합니다.

- 결과가 정해져 있는 논리 연산에서 굳이 불필요한 연산은 하지 않도록 하기 위해 사용하는 방법 입니다.

<br>

### 예시
```js
const MailBox = (props) => {
    const unreadMessages = props.unreadMessages;

    return (
        <div>
            <h1>안녕하세요!</h1>
            {unreadMessages.lenght > 0 &&
                <h2>
                    현재 {unreadMessages.lenght} 개의 읽지 않은 메시지가 있습니다.
                </h2>
            }
        </div>
    )
}
```

- 위의 코드를 보면 조건문 unreadMessages.lenght > 0 에 따라서 뒤에 나오는 값 `<h2>` 태그로 둘러싸인 부분이 렌더링이 되거나 안되거나 합니다.
- __&&__ 연산자를 사용하는 이러한 패턴은 단순하지만 리액트에서 굉장히 자주 많이 사용되는 패턴 입니다.

***
<br>

## 인라인 조건 ( if-else )
if 는 보여주거나, 안 보여주는 두 가지 경우만 있었지만 <br>
if-else 는 조건문의 값에 따라 다른 엘리먼트를 보여줄 때 사용 합니다.<br>

- 이것을 흔히, __삼항 연산자__ 라고 불리는 ? 를 사용 합니다.

<br>

> 조건문 ? 참일 경우 : 거짓을 경우

<br>

### 예시 1
```js
const UserStatus = (props) => {
    return (
        <div>
            사용자는 현재 {props.isLoggedIn ? '로그인' : '로그인 하지 않은'} 상태 입니다.
        </div>
    )
}
```
- isLoggedIn 의 값이 true 인 경우는 '로그인' 문자열을 출력하고
- false 인 경우는 '로그인 하지 않은' 문자열을 출력 합니다.

<br>

### 예시 2
```js
const LoginControl = (props) => {
    const [isLoggedIn, setIsLoggedIn] = useState(false);

    const handleLoginClick = () => {
        setIsLoggedIn(true);
    }

    const handleLogoutClick = () => {
        setIsLoggedIn(false);
    }

    return (
        <div>
            <Greeting isLoggedIn={isLoggedIn}/>
            {isLoggedIn ? 
                <LogoutButton onClick={handleLogoutClick}/>
                :
                <LoginButton onClick={handleLoginClick}/>
            }
        </div>
    )
}
```

- 위와 같이 문자열이 아닌 엘리먼트를 넣어서 사용 할 수 있습니다.

<br>

## 컴포넌트 렌더링 막기
지금까지 조건부 렌더링을 하는 방법에 대해서 공부 했는데, 만약 __컴포넌트를 렌더링하고 싶지 않을 때__ 가 생긴다면 ? <br>
__null__ 리턴해주면 됩니다.

- 리액트는 null 을 리턴하면 렌더링 되지 않기 때문 입니다.

***
<br>

## 참고
- 소풀의 처음 만난 리액트