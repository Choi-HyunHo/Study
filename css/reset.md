# Resetting과 Normalizing CSS의 차이점

웹 브라우저마다 각자의 HTML요소의 기본 스타일을 가지고 있습니다.<br> 
CSS 스타일링을 적용할때 이러한 기본 스타일링들이 방해를 할 뿐더러, 브라우저마다 다른 HTML 요소에 대한 기본 값 때문에 일어나는 __크로스 브라우징__ 을 막기 위해서 입니다.

<br>

## Reset CSS
- 완전히 CSS 를 아무것도 없는 백지처럼 초기화 합니다.
- 주소 : https://meyerweb.com/eric/tools/css/reset/

<br>

### 장점
- 다른 부분에 신경을 쓰지 않아도 되는 장점이 있습니다. ->  모든 것을 reset하고 시작하기 때문에 고려해야할 변수가 적습니다.

<br>

### 단점
- 적용되는 스타일이 하나도 없기 때문에 오히려 코드의 길이가 길어질 수도 있습니다.

- 브라우저는 계속 해서 업데이트되고 있는 반면, 리셋 CSS의 최근 업데이트는 2011년 입니다.<br>
  그만큼 오래된 자료이고, 유용한 스타일도 제거하기 때문에 비효율적이라는 비판도 존재 합니다.

<br>

## Normalize CSS
- 기존의 모든 스타일을 제거하는 Reset과 달리 Normalize는 이를 유지하고 어느 정도 유용한 스타일들은 이용하려는 스타일링 방법 입니다.
- 주소 : https://necolas.github.io/normalize.css/

<br>

### 장점
- Reset과 다르게 github을 통해 지속적인 업데이트가 이루어지고 있기 때문에 업데이트가 없는 Reset에 비해서 안정성이 높습니다.


<br>

## 정리
- Resetting은 요소의 __모든 기본 브라우저 스타일을 제거__ 하기 위한 CSS 입니다.
- Normalizing은 모든 스타일을 제거하는 것이 아니라 __유용한 기본 스타일을 보존__ 합니다.<br> 
  또한, 일반적인 브라우저 종속성에 대한 버그를 수정 합니다.


<br>

## 참고
- https://codechasseur.tistory.com/105
- https://padro.tistory.com/190
- https://velog.io/@cjy0029/Normalize-Reset-%EB%AD%98%EC%8D%A8%EC%95%BC-%ED%95%A0%EA%B9%8C