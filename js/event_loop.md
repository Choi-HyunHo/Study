# Event Loop

먼저, 자바스크립트 엔진에 대해 알아야 합니다. 

<br>

## 자바스크립트 엔진
- 자바스크립트 엔진 안에는 `Memory Heap` 과 `Call Stack` 로 나뉘어 있습니다. 

<img src="https://user-images.githubusercontent.com/87301268/160239990-cf1b1c3d-0297-4892-a078-0f644a506c4a.png" width=700px>

- Memory Heap 
    - 데이터를 만들 때, 즉 변수를 선언해서 오브젝트를 할당하거나 문자 또는 숫자를 할당하게 되면 전부 메모리 힙에 저장 됩니다.
    - __메모리 할당이 일어나는 곳__

- Call Stack 
    - 함수를 실행하는 순서에 따라서 차곡차곡 쌓아놓는 것 
    - __코드 실행에 따라 호출 스택이 쌓이는 곳__
    - LIFO(Last In First Out) : 제일 나중에 들어온 것이 제일 처음으로 나갑니다.

<br>

## 런타임 환경
- 자바스크립트 언어 자체에는 멀티 스레딩이 없습니다.

<img src="https://user-images.githubusercontent.com/87301268/160240996-fe95ac66-a9a0-4dab-b77d-5480fb2c0edb.png" width=700px>

<br>

### Process 
- __운영체제 위에서 연속적으로 실행되고 있는 프로그램__
- 독립적으로 메모리에서 실행 합니다.

- 각각의 프로세스는 자원(리소스)들이 정해져 있습니다.
- 프로세스 안에는
    - `code` : 프로그램을 실행하기 위한 코드
    - `stack` : 프로세스 안에서 함수들이 어떤 순서로 실행되어야 하는지, 함수가 끝나면 어디로 다시 돌아가는지에 대한 정보
    - `Heap` : 오브젝트를 생성하거나 데이터를 만들 때, 데이터들이 저장되는 공간 [ 동적으로 할당된 변수가 저장 ]
    - `Data` : 전역 변수나 스태틱 변수들이 할당

<br>

### Thread
- __한 프로세스 안에서 여러 개 동작할 수 있는데 각각 저마다 해야 되는 임무를 받습니다.__

- 즉, 한 프로세스 안에서 동시에 기능을 수행하는 여러 개의 Thread가 생성

- 예시 ) 프로그램에서 음악을 들으면서 사진을 편집할 수 있는 어플리케이션이 있다면,<br> 
각각 스레드는 음악을 재생하는 스레드와 사진을 편집하는 스레드가 있습니다.

<br>

### JavaScript 는
- 기본적으로 싱글 스레드 기반 언어 입니다.

- 즉, 호출 스택이 하나. 따라서 한 번에 하나의 작업만 처리가 가능 합니다.
- 하지만, 자바스크립트가 동작하고 있는 브라우저 위에는 즉, 브라우저라는 프로그램 안에서는 여러 가지의 스레드가 들어있습니다.

-  web API's 와 Event Loop 등을 사용하여 멀티 스레딩이 가능하다.

<br>

## Event Loop
1. Call Stack 과 Task Queue의 상태를 체크 보면서 Call Stack이 실행 중이면 기다렸다가 
2. Call Stack의 함수가 모두 실행되고 완전히 비워지면 
3. Task Queue의 첫 번째 callback 함수를 Call Stack에 하나씩 추가하여 

4. 자바스크립트 엔진이 차례대로 수행할 수 있도록 해줍니다.

<br>

### Task Queue
- 자바스크립트 런타임 환경에서 비동기적으로 실행되는 callback 함수를 보관하는 공간

- Call Stack 이 빈 상태가 되면, `Event Loop` 에 의해 Task Queue에 저장된 순서대로 하나씩 Call Stack으로 이동 합니다.

<br>

## 예시
```js
function first(){
    console.log('first')
}

function second(){
    console.log('second')
}

function thrid(){
    console.log('thrid')
}

first()
setTimeout(()=> second(),2000)
thrid()
```

1. first() 가 호출되면서 call stack 에 추가되고, fisrt() 실행

<img src="https://user-images.githubusercontent.com/87301268/160241931-f2cab7dc-7860-41dc-bb66-ae1709e9e56d.jpg" width=500px height=300px>

2. console에 first가 출력되고, first() 실행이 끝나면 call stack 에 first() 가 삭제되고 비워집니다.

<img src="https://user-images.githubusercontent.com/87301268/160242264-e073544f-b379-4ead-91f6-a01ad2ee8e57.jpg" width=500px height=300px>

3. setTimeout(()=> second(),2000) 호출되고, call stack에 추가

<img src="https://user-images.githubusercontent.com/87301268/160242311-db76aeb8-b1f3-49c8-adde-d27479fd9db3.jpg" width=500px height=300px>

4. setTimeout()이 실행되면서 브라우저가 제공하는 web API를 호출하고 call stack에서 제거. web API 에서는 Timer가 실행

<img src="https://user-images.githubusercontent.com/87301268/160270254-4f893d47-7b5d-4527-abb1-6200a10ccf0b.jpg" width=500px height=300px>

5. web API 에서는 timer 가 실행되는 동안 thrid() 가 호출되고, third()가 call stack에 추가

<img src="https://user-images.githubusercontent.com/87301268/160270277-8558a2dd-f3dc-4ce8-b465-b6a0c9af1725.jpg" width=500px height=300px>

6. console에 third가 출력되고, thrid() 실행이 끝나면 call stack 에 thrid() 가 삭제되고 비워집니다.

<img src="https://user-images.githubusercontent.com/87301268/160270282-73c8cb76-63ca-42cf-8933-605e8c37211e.jpg" width=500px height=300px>

7. 2000ms 가 지나 timer가 종료되면 callback 함수 second()를 Task Queue에 저장

<img src="https://user-images.githubusercontent.com/87301268/160270291-2ac54ead-1ab9-465b-ad55-0538a16cf119.jpg" width=500px height=300px>

8. Event Loop는 call stack이 비어있는 것을 확인하고 Task Queue에 있는 함수를 call stack에 추가

<img src="https://user-images.githubusercontent.com/87301268/160270300-d581149c-7ee6-41f6-8fd8-6b8c32c82425.jpg" width=500px height=300px>

9. second()가 실행되어 console에 second가 출력되고, second() 실행이 끝나면 call stack 에 second()가 삭제되고 비워집니다.

<img src="https://user-images.githubusercontent.com/87301268/160270308-ef507ba8-7fac-42b7-aff6-65a624d1c96e.jpg" width=500px height=300px>

<br>

## 참고
- [코드 안식처](https://blog.naver.com/x7788/222639066315)
- [캡틴판교](https://joshua1988.github.io/web-development/translation/javascript/how-js-works-inside-engine/#%EB%8F%99%EC%8B%9C%EC%84%B1concurrency--%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84event-loop)
- [자바스크립트 런타임 환경](https://velog.io/@ouo_yoonk/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%9F%B0%ED%83%80%EC%9E%84%ED%99%98%EA%B2%BD)
- [자바스크립트 동작 원리](https://designer-ej.tistory.com/entry/Web-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-JavaScript-Runtime-Environment?category=960976)
- https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf