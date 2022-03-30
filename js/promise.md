# Promise

자바스크립트에서 제공하는 비동기를 간편하게 처리 할 수 있도록 도와주는 객체

- 정해진 장시간의 기능을 수행하고 나서 정상적으로 기능이 수행이 되어지면<br>성공의 메세지와 함께 처리된 결과 값을 전달 합니다.

- 만약, 기능을 수행하다가 예상치 못한 문제가 발생하면 `error` 를 전달 합니다.

<br>

## 사용하는 이유

- 네트워크에서 데이터를 받아오거나 큰 데이터를 읽어오는 과정은 시간이 걸리는데 동기적으로 처리하면<br> 앞선 일들을 처리하는 동안 다음 라인의 코드가 실행되지 않기 때문에 시간이 걸리는 일 들은 <br>`promise` 를 통해 비동기적으로 사용하는 것이 좋습니다.

<br>

## promise 의 상태
- promise는 state와 result 2가지 숨김 프로퍼티를 갖고 있습니다.<br>
result는 resolve(value)가 호출되면 value가 되며, reject(error)가 호출되면 error를 갖는다.

    - `pending` : 프로미스가 만들어져서 우리가 지정한 오퍼레이션이 수행 중일 때

    - `fulfilled` : 오퍼레이션을 성공적으로 끝내게 되면
    - `rejected` : 파일을 찾을 수 없거나 네트워크의 문제가 생기면


<br>

## promise 과정

### 시작하기 
- class 이기 때문에 `new` 라는 키워드를 이용해서 오브젝트를 생성

- `promise` 는 executor 라는 콜백함수를 받고 그 안에는 `resolve`, `reject` 라는 콜백 함수가 있습니다.

    - `resolve` : 기능을 정상적으로 수행해서 마지막에 최종적으로 데이터를 전달하는 것

    - `reject` : 기능을 수행하다가 중간에 문제가 생기면 호출하게 되는 것

- 주의점
    - promise 를 만드는 순간 executor 라는 콜백 함수가 바로 실행이 되기 때문에 유의하며 사용
```js
const promise = new Promise((resolve, reject)=>{
    // heavy work (network, read files)
    console.log('doing someting...')
    setTimeout(() => {
        resolve('hyunho')
    }, 2000)
})

// promise 는 어떠한 일을 2초 정도 수행 하다가 결국 잘 마무리 되면 resolve 라는 콜백함수를 호출하면서 'hyunho'라는 값을 전달
```
<br>

### 사용하기
- `then` : 값이 정상적으로 잘 수행이 되면[then] 어떤 `value` 를 받습니다. 그리고 우리가 원하는 기능을 수행하는 콜백 함수를 전달해 주면 됩니다.
```js
promise.then((value)=>{ 
    console.log(value) // 2초 뒤에 hyunho 출력
})

//  value 는 promise 가 정상적으로 잘 수행이 되어서 마지막으로 resolve 콜백함수에서 전달된 'hyunho'라는 값이 들어갑니다.
```
<br>

### 만약 `reject` 를 위에서 사용하면 ?
```js
const promise = new Promise((resolve, reject)=>{
    // heavy work (network, read files)
    console.log('doing someting...')
    setTimeout(() => {
        //resolve('hyunho')
        reject(new Error('no network')) // Error 는 JS에서 제공하는 오브젝트
    }, 2000)
})

promise.then((value)=>{ 
    console.log(value) // 2초 뒤에 no network 출력
})
```
![캡처 (1)](https://user-images.githubusercontent.com/87301268/160792580-f13cedfb-4d05-4c2e-8e09-ed9f23424354.jpg)

<br>

- `catch` : 에러가 발생 했을 때 어떻게 처리 할 것 인지 콜백 함수 등록
    - __더 이상 에러가 발생하지 않고 우리가 받아온 에러 메세지__ 가 출력 됩니다.

```js
promise
    .then((value)=>{ // value 는 promise 가 정상적으로 잘 수행이 되어서 마지막으로 resolve 콜백함수에서 전달된 'hyunho'라는 값이 들어간다.
         console.log(value)
    })
    .catch(error =>{
        console.log(error)
    })
```
![캡처 (2)](https://user-images.githubusercontent.com/87301268/160793545-95d530ff-e77c-4f93-9aef-2b597a304d7a.jpg)

<br>

- `finally` : 성공하든 실패하든 상관 없이 무조건 마지막에 호출
```js
promise
    .then((value)=>{ // value 는 promise 가 정상적으로 잘 수행이 되어서 마지막으로 resolve 콜백함수에서 전달된 'hyunho'라는 값이 들어간다.
         console.log(value)
    })
    .catch(error =>{
        console.log(error)
    })
    .finally(()=>{
        console.log('finally')
    })
```

<br>

## promise 연결하기
```js
const fetchNumber = new Promise((resolve, reject)=>{
    setTimeout(()=> resolve(1), 1000)
})

fetchNumber
    .then(num => num * 2) // 1이 num에 저장 
    .then(num => num * 3) // 2가 num에 저장
    .then(num =>{ // 6이 num에 저장
        return new Promise((resolve, reject)=>{
            setTimeout(()=>{
                resolve(num - 1) 
            }, 1000)
        })
    })
    .then(num => console.log(num)) // 5 출력
```
- `then` 은 값을 바로 전달 할 수도 있고, __promise 를 전달 할 수 있습니다.__

<br>

## 오류처리 하는 방법
```js
const getHen = ()=>
    new Promise((resolve, reject)=>{
        setTimeout(()=>
            resolve('🐔')
        ,1000)
    })


const getEgg = hen =>
    new Promise((resolve, reject)=>{
        setTimeout(()=>
            reject(new Error(`${hen}=>🥚`))
        ,1000)
    })


const cook = egg =>
    new Promise((resolve, reject)=>{
        setTimeout(()=>
            resolve(`${egg}=>🍳`)
        ,1000)
    })


getHen() 
    .then(getEgg)
    .catch(error =>{ // getEgg를 받아올 때 문제가 생긴다면
        return '🍕'
    })
    .then(cook)
    .then(console.log)
    .catch(console.log
```
1. egg 부분에 문제가 있지만 다른 음식으로 대체 하고 싶을 경우
2. egg를 받을 수 없지만 피자로 대신하여 promise 가 잘 실행 됩니다.

3. __문제가 있는 곳 앞에 바로 `catch`__ 를 사용하면 됩니다.

<br>

## 정리
- 전통적인 자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용 합니다. 그런데 이런 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고, <br>
비동기 처리 중 발생한 에러의 처리가 곤란하여 ES6부터 비동기 처리를 위한 Promise가 도입 되었습니다.

- promise는 비동기 처리에 성공하면 `resolve 메소드`를 호출해서 비동기 처리 결과를 `후속처리 메서드(then)`로 전달

- promise는 비동기 처리에 실패하면 `reject 메소드`를 호출해서 에러메시지를 `후속처리 메서드(catch)`로 전달




<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222641411948)
- [드림코딩](https://www.youtube.com/watch?v=JB_yU6Oe2eE&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=12)
- [MDN, promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- https://ko.javascript.info/promise-basics