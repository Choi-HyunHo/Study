# Async / Await

`async/await` 를 사용하면 비동기 코드를 작성할 때 비교적 쉽고 명확하게 코드를 작성할 수 있습니다.

<br>

## promise 사용 하기 전 (문제점)
```js
function fetchUser(){
    // 데이터를 받아오는데 10초 정도 걸린다고 가정
    return 'hyunho';
}

const user = fetchUser()
console.log(user)
```
- 위에처럼 시간이 걸리는 코드를 비동기적으로 처리를 하지 않으면, 자바스크립트 엔진은<br>
동기적으로 코드를 수행하기 때문에, 즉 한줄 한줄씩 한줄이 끝나야 그 다음 줄로 넘어 갑니다. 

- fetchUser() 함수가 선언된 곳으로 가서 함수의 코드 블록을 실행하는데 데이터를 받아오는데<br>
10초가 걸리니까 10초동안 머무르고 있고, 그리고 나서야 'hyunho' 가 리턴이 됩니다.

- 결론 : 비동기 처리를 하지 않으면 사용자의 데이터를 받아오는데 10초가 걸리기에 만약 console.log(user) 뒤로 <br>코드들이 계속 된다면 10초가 끝날 때 까지 실행되지 않고 대기해야 합니다.

<br>

## promise 사용
```js
function fetchUser(){
    // 데이터를 받아오는데 10초 정도 걸린다고 가정
    return new Promise((resolve,reject)=>{
        resolve('hyunho');
    })
}

const user = fetchUser() // fetchUser가 promise 를 리턴
user.then(console.log)
```

<br>

## async
- 자동적으로 함수 안에 있는 코드 블럭 들이 promise로 변환이 됩니다.
- 사용하는 방법은 함수 앞에 `async` 키워드를 연결 합니다.
```js
async function fetchUser(){
    // 데이터를 받아오는데 10초 정도 걸린다고 가정
    return 'hyunho'
}

const user = fetchUser()
user.then(console.log)
```

<br>

## await
- 자바스크립트는 await 키워드를 만나면, 프로미스가 처리 될 때 까지 기다립니다.

- 사용하는 방법은 `async` 가 붙은 함수 안에서만 사용할 수 있고, `await` 뒤에 붙은 코드를 기다립니다.
```js
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```
```js
function delay(ms){
    return new Promise(resolve => setTimeout(resolve,ms))
    // 정해진 ms 가 지나면 resolve 리턴
}

async function getApple(){
    await delay(1000) // delay가 끝날 때 까지 여기서 기다립니다.
    return '🍎'
}

async function getBanana(){
    await delay(1000)
    return '🍌'
}

async function pick(){
    const apple = await getApple(); // apple를 받고
    const banana = await getBanana(); // banana를 받고
    return `${apple} + ${banana}` // 다 받은 후 실행
}

// async / await 를 사용하지 않고 기존의 promise 만 이용하는 경우
function pick(){
    return getApple()
    .then(apple => {
        return getBanana()
        .then(banana => `${apple} + ${banana}`)
    })
}

```
<br>

## 예외처리 ( try...catch )
```js
function delay(ms){
    return new Promise(resolve => setTimeout(resolve,ms))
    // 정해진 ms 가 지나면 resolve 리턴
}

async function getApple(){
    await delay(1000) // delay가 끝날 때 까지 여기서 기다립니다.
    return '🍎'
}

async function getBanana(){
    await delay(1000)
    return '🍌'
}

async function pick(){
    try{
        const apple = await getApple(); // apple를 받고
        const banana = await getBanana(); // banana를 받고
    } catch(error) {
        console.log(error)
    }
    return `${apple} + ${banana}` // 다 받은 후 실행
}

```




<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222649418081)
- [드림코딩](https://www.youtube.com/watch?v=aoQSOZfz3vQ&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=13)
- [캡틴판교](https://joshua1988.github.io/web-development/javascript/js-async-await/)
- [MDN, promise](https://developer.mozilla.org/ko/docs/conflicting/Learn/JavaScript/Asynchronous/Promises)