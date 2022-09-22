# 01_프로그래밍
## 1.1 프로그래밍이란?
- 컴퓨터에게 실행을 요구하는 <span style="color:#0000FF">일종의 커뮤니케이션</span>	
-> 이를 위해 먼저 무엇을 실행하고 싶은지 정의할 필요가 있다.
-> 1. 프로그래밍에 앞서 해결해야 할 문제(요구사항)을 명확히 이해하고
-> 2. 적절한 문제 해결 방안을 정의할 필요가 있다.

- 이 때 요구되는 것이 __문제 해결 능력__ 이다.

<br>

### 하지만 대부분의 문제는 명확하지 않을 수 있다. 따라서..
1. 문제(요구사항) 를 명확히 이해하는 것이 우선되어야 하며
2. 복잡함을 단순하게 분해 하고
3. 자료를 정리하고 구분해야 하며
4. 순서에 맞게 행위를 배열해야 한다.

> 즉, __프로그래밍 이란__ 
0과 1 밖에 알지 못하는 기계가 실행 할 수 있을 정도로 __정확하고 상세하게 요구사항을 설명__ 하는 작업이며, 그 결과물이 바로 코드다.
   

***
<br>

## 1.2 프로그래밍 언어
- 문제 해결 능력을 바탕으로 정의된 문제 해결 방안은 컴퓨터에게 전달되어야 한다.
- 이유는 명령을 수행 할 주체는 컴퓨터이기 때문이다.
- 따라서, 사람이 이해 할 수 있는 자연어가 아니라 컴퓨터가 이해 할 수 있는 언어
<span style="color:#0000FF"> 기계어 </span> 로 전달해야 한다.

<br>

### 하지만 사람이 기계어를 이해해서 직접 명령을 전달하는 것은 매우 어렵다.
- 그래서 가장 유용한 대안은 사람이 이해 할 수 있는 약속된 __구문(문법)__ 으로 구성된
__'프로그래밍 언어'__ 를 사용하여 프로그램을 작성 하고
- 그것을 컴퓨터가 이해 할 수 있는 기계어로 변환하는 일종의 번역기를 사용하는 것이다.
- 번역기를 __컴파일러__ 또는 __인터프리어__ 라고 한다.

![](https://velog.velcdn.com/images/hoho_0815/post/de0a1575-e95a-4271-9b19-24b27c849b4c/image.png)

> 프로그래밍은 프로그래밍 언어를 사용해 컴퓨터에게 실행을 요구하는 __커뮤니케이션__ 이다.
프로그래밍 언어는 __구문__ 과 __의미__ 의 조합으로 표현된다.

***
<br>

## 1.3 구문과 의미
### 서론
- 프로그래밍을 배우는 것은 일반적으로 프로그래밍 언어의 문법을 배우는 것부터 시작한다.
- 이는 외국어 학습과 유사 하지만, 문법을 잘 안다고 외국어를 잘한다고 할 수는 없다.
- 결론적으로 외국어를 잘하려면 말이나 문장을 정확히 이해한 후, 문맥에 따른 적절한 어휘 선택, 그리고 순차적으로 결론을 향해 나아가는 문장 구성이 필요하다.
- 즉, 문법에 맞는 문장을 구성하는 것은 물론 __의미__ 를 가지고 있어야 언어의 역할을 충실히 수행 할 수 있다.

<br>

### 예시
```js
const number = 'string';
console.log(number * number); // NaN
```
- 자바스크립트의 변수에는 어떠한 타입의 값도 할당할 수 있다.
- 따라서 위 예시는 문법적으로는 전혀 문제가 없지만, 의미적으로는 옳지 않다.
- __number__ 라는 변수에 문자가 할당되어 있기 때문이다.
- __number__ 라는 변수에는 숫자를 할당하는 것이 의미적으로 옳다.

***
<br>

## 정리

- 즉, 작성된 코드는 해결 방안의 구체적 구현물이다.
- 이것은 프로그래밍 언어의 문법에 부합하는 것은 물론이고 수행하고자 하는 바를 정확히 수행하는 것, 즉 __요구사항이 실현(문제가 해결)__ 되어야 의미가 있다.