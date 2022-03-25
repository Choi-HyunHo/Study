# 실행 컨텍스트(Execution Context)
추상적인 개념<br>

실행할 코드에 제공할 환경 정보들을 모아놓은 객체
<br>

## Stack, Queue

- Stack: LIFO (Last In First Out)
- Queue: FIFO (First In First Out)

<img src="https://user-images.githubusercontent.com/87301268/160141809-bb7ef4ad-7e9e-47cc-823b-f14c94b044bd.png" width=500px>

- `Stack` 에서는 가장 마지막에 들어온 d,c,b,a 순으로 데이터를 꺼낼 수 있습니다.

- `Queue` 에서는 제일 먼저 들어온 a,b,c,d 순으로 데이터를 꺼낼 수 있습니다.

>> __실행 컨택스트의 동작은__, 동일한 환경에 있는 코드들을 실행할 때 필요한 환경정보를 모아 컨텍스트를 구성하고,<br> 이를 콜 스택에 쌓아놓은 뒤, 가장 위에 있는 컨텍스트와 관련 있는 코드들을 실행하는 것으로 코드의 환경과 순서를 보장합니다.

<br>

## 실행 컨텍스트의 구성 방법
- 전역 공간 → 자동 실행
- eval() 함수 → 사용을 권장하지 않음
- 함수 → 가장 흔한 실행 컨텍스트 구성 방법
- 블록 → { } 로 둘러 쌓인 코드 내부. ES6 부터 지원

<br>

## 실행 컨텍스트와 콜 스택
```js
// ------------------- (1)
var a = 1;
function outer() {
  function inner() {
    console.log(a);
    var a = 3;
    // --------------- (2)
  }
  inner(); // -------- (3)
  console.log(a);
  // ----------------- (4)
}
outer(); // ---------- (5)
console.log(a);
// ------------------- (6)
```
- 실행 순서 : (1) → (5) → (3) → (2) → (4) → (6)

<img src="https://user-images.githubusercontent.com/87301268/160144887-3d5fd212-3a5e-448a-a20a-1cc47aa22f8b.png" width=700px>

1. 프로그램 실행: [전역 컨텍스트]
2. outer 실행: [전역 컨텍스트, outer] -> (5)
3. inner 실행: [전역 컨텍스트, outer, inner]-> (3)
4. inner 종료: [전역 컨텍스트, outer] -> (2)

5. outer 종료: [전역 컨텍스트] -> (4)
6. 프로그램 종료

<br>

## 실행 컨텍스트의 구조
<img src="https://user-images.githubusercontent.com/87301268/160146223-cd9b56ed-54ce-4b2e-862c-a19cba1ebcc3.jpg" width = 800px>

- 실행 컨텍스트 객체는 활성화되는 시점에 `VariableEnvironment`, `LexicalEnviornment`, `ThisBinding` 세 가지 정보 수집합니다.

<br>

## Variable Environment
- 실행 컨텍스트를 생성할 때, VariableEnvironment에 정보를 먼저 담은 후 이를 그대로 복사해서 LexicalEnvironment를 만들고<br> 이후에 LexicalEnvironment를 주로 활용하게 됩니다. 실행중에도 변경사항이 반영되지 않으며 초기 상태를 유지합니다.

- 내부 구성 요소
    - 현재 컨텍스트 내의 식별자(변수)들에 대한 정보
    - 외부 환경 정보
    - 선언 시점의 LexicalEnvironment의 스냅샷 (변경사항 반영 X)

<br>

## Lexcial Environment
- LexicalEnvironment의 내부에는 environmentRecord와 outerEnvironmentReference로 구성되어 있습니다.

- environmentRecord로 인하여 호이스팅이 발생
- outerEnvironmentReference로 인하여 스코프와 스코프체인이 형성


<br>

## thisBinding
- 식별자가 바라봐야 할 대상 객체

<br>

## 정리
- 실행할 코드 제공할 환경 정보들을 모아 놓은 객체입니다.
- 실행 컨텍스트 객체는 활성화 되는 시점에 VariableEnvironment, LexicalEnvironment, ThisBinding의 세 가지 정보 수집합니다.
- VariableEnvironment와 LexicalEnvironment는 environmentRecord(매개 변수명, 식별자, 함수명등 수집)와<br> 바로 직전 컨텍스트의 LexicalEnvironment 정보를 참조하는 outerEnvironmentReference로 구성됩니다.
- 실행 컨텍스트 생성할 때, VariableEnvironment는 초기상태 유지, LexicalEnvironment는 실행 도중 변경사항 즉시 반영됩니다.

<br>

## 참고
- [실행_컨텍스트_구성](https://junilhwang.github.io/TIL/Javascript/Domain/Execution-Context/#_2-%E1%84%89%E1%85%B5%E1%86%AF%E1%84%92%E1%85%A2%E1%86%BC-%E1%84%8F%E1%85%A5%E1%86%AB%E1%84%90%E1%85%A6%E1%86%A8%E1%84%89%E1%85%B3%E1%84%90%E1%85%B3-%E1%84%80%E1%85%AE%E1%84%89%E1%85%A5%E1%86%BC)
- https://taenami.tistory.com/109
- https://jinbroing.tistory.com/78
- https://velog.io/@tess/Execution-Context-Call-Stack