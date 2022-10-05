# 개발 시간을 줄여주는 이름 짓기와 주석 쓰기
## 1. 개발자의 가장 큰 고민은 이름 짓기
모든 개발자는 자기 코드를 읽는 사람이 주석 없이도 금방 이해하게 코드를 작성하고 싶어 한다.<br>
-> 함수가 어떤 일을 하는지 이름만 보고도 짐작하게 만들고 싶고, 간결하고 명료하게 일관성 가진 이름을 짓고 싶다.

<br>

### 1.1 이름 짓기는 창조가 아니라 조합
개발자들이 이름 짓기를 어려워하는 가장 큰 이유는 무에서 유를 창조해야 한다고 생각하기 때문이다. <br>
-> 하지만 전혀 그렇지 않다.

<br>

> 이름 짓기는 창조 과정이 아니라 정해진 원칙으로 적절한 단어를 선택해 조합하는 과정일 뿐 

<br>

- 예를 들어, 칸막이에 짬뽕과 짜장면을 각각 담은 메뉴를 짬짜면이라 부르듯..
- 모두 기존 이름을 조합

<br>

### 1.2 코드의 네이밍 컨벤션은 영어 표기법을 상속받았다.
네이밍 컨벤션은 기본적으로 영어의 표기법을 준수

<br>


#### 1.2.1 파스칼 표기법으로 클래스 이름 짓기
__파스칼 표기법은 모든 단어에서 첫 글자를 대문자로 쓰는 방식이다.__ -> 주로 클래스 이름에 사용 <br>
이유는, 클래스가 프로그래밍에서 가장 주요하고 높은 위치에 있고, 고유명사처럼 특정되며, 명사로 돼 있기 때문이다.

<br>

#### 1.2.2 카멜 표기법으로 함수 * 변수 이름 짓기
__카멜 표기법은 첫 단어를 빼고, 나머지 단어의 첫 번째 글자만 대문자로 쓴다.__ -> 주로 함수나 변수에 사용

<br>

잘못된 예
```js
int TotalCount = 0;
void OrderCoffee();
```

<br>

좋은 예
```js
int totalCount = 0;
void orderCoffee();
```

<br>

#### 1.2.3 상수는 모두 대문자로 쓴다.
프로그래밍할 때는 소문자를 쓰는 변수와 구별하기 위해 상수를 모두 대문자로 쓰고 언더스코어(_)로 단어를 연결한다. <br>
-> 상수는 __값이 변해서는 안된다는 점을 강조__ 하고, 주의시키기 위해 가독성을 높이는 방법으로 선택

```js
static final int COFFEE_MAX = 10;
```

<br>

#### 1.2.4 패키지와 모듈은 모두 소문자로 쓴다.
패키지와 모듈은 분명히 클래스나 함수보다 더 높은 위치다. 그러므로 당연히 대문자로 써야 할 것 같다. <br>
- 하지만 실제로는 __소문자로만 쓴다.__ 
- 패키지와 모듈이 클래스를 모으거나 함수를 담아놓은 통에 불과하기 때문이다.

<br>

안 좋은 예
```js
kr.co.wikibook.android.DeveloperWriting
import DeveloperWriting
```

<br>

좋은 예
```js
kr.co.wikibook.android.developerwriting
import developerwriting
```

<br>

#### 1.2.5 BEM 표기법
CSS 에서 사용하는 BEM 표기법은 '대상__요소--상태' 를 의미한다.
- __대상의 요소나 부분__ 을 의미할 때는 언더스코어 두 개(__)
- 대상이나 요소의 __상태나 속성을 의미할 때는 하이픈 두 개(--)__ 로 연결

<br>

```js
.form {} // 대상
.form_button {} // 대상__요소
.form_button--disabled {} // 대상__요소--상태 
```

<br>

> 수 많은 표기법이 있지만 가장 중요한 것은 가독성과 소통

***
<br>

## 2. 변수 이름을 잘 짓는 법
### 안 좋은 예
```js
int d
int m
int y
```
a 나 b 와 같은 아무런 의미가 없는 글자를 변수로 쓰는 것은 좋지 않다.
- 예를 들어 d 는 일자를 d 로 표기할 때가 많다.(day)
- 하지만 어떤 개발자는 d 를 보고 date 나 double 를 떠올릴지도 모른다.

<br>

### 좋은 예
```js
int someday
int today
int thisMonth
int finalYear
```

<br>

### 2.1 중요한 단어를 앞에 쓴다.
변수 이름을 여러 단어로 조합할 때는 순서를 잘 정해야 한다. <br>
예를 들어 총 방문자 수를 나타내는 변수를 보통 __totalVisitor__ 로 그대로 번역해 변수로 사용하곤 한다.<br>

하지만, 이 변수를 다시 사용할 때는 total로 검색을 시작하는 경우보다 visitor로 검색을 시작하는 경우가 더 많을 것이다. <br>
따라서, total 이라는 수식어보다 __본래 의미를 뜻하는__ visitor 를 앞에 쓸 것을 추천한다.

<br>

#### 2.1.1 안 좋은 예
```js
int totalVisitor
int totalRegister
int totalBuyer
int numberOfTotalVip
```

<br> 

#### 2.1.2 좋은 예
```js
int visitorTotal
int registerTotal
int buyerTotal
```

- 물론 요즘 개발 도구는 검색할 때 해당 단어가 포함된 클래스나 변수를 다 찾아준다.
- 그래서 단어를 조합해 이름을 지을 때 순서가 그다지 의미 없을 수도 있다.
- 하지만, __중요한 것이 앞에 와야한다는 기본 원칙__ 은 지키자.

<br>

### 2.2 함수 이름 짓는 순서
회원가입을 하는 웹페이지에서 다음과 같은 기능이 있어야 한다고 가정 <br>

> "사용자가 이름을 입력하고 등록 버튼을 클릭하면, 시스템이 사용자 이름을 input 태그에서 가져와 이름 입력 여부와 글자 수를 확인한 후 입력이 안 되었으면,<br> 스크립트를 중단하고 input 태그를 활성화해 사용자가 쓸 수 있게 하고, 글자 수가 한글 두 글자 이하면 확인을 요청해 사용자가 확인할 수 있게 한다."


<br>

- 이를 모두 함수 이름으로 옮길 수 없다.
- __함수는 시스템이 할 일을 나타내는 것__ , 사용자가 할 일을 나타내는 것은 아니다.
- 따라서, 함수의 주체, 즉 주어가 필요없다.
- 논리적으로 합쳐야 하거나 떼야 할 것을 확인해서 다시 정리

<br>

1. 사용자 이름을 input 태그에서 가져온다.
2. 사용자 이름의 글자 수를 확인한다.
3. 입력이 안 되었으면, input 태그를 활성화한다.
4. 글자 수가 한글 두 글자 이하면 확인을 요청한다.

<br>

#### 2.2.1 함수는 1 함수 1 업무 규칙을 적용
위의 3,4 번은 2번에 속하는 동작이므로 2~4 번을 함수 하나로 묶을 수 있다. <br>

1. (함수1) 사용자 이름을 input 태그에서 가져온다.
2. (함수2) 사용자 이름의 글자 수가 2글자 이하면 다음과 같이 처리한다.
    - 만약, 글자 수가 0(null) 이면, input 태그를 활성화한다.
    - 만약, 글자 수가 1 or 2 이면, 사용자에게 확인을 요청한다.

<br>

#### 2.2.2 함수 문장을 영어로

1. (함수1) 사용자 이름을 input 태그에서 가져온다.
    - get user's name from the text input field
2. (함수2) 사용자 이름의 글자 수가 2글자 이하면 다음과 같이 처리한다.
    - do something if user's name contains under 2 characters

<br>

#### 2.2.3 영문에서 불필요한 단어를 빼고, 의미있는 단어만 남기자

1. (함수1) 사용자 이름을 input 태그에서 가져온다.
    - get user name from input field
2. (함수2) 사용자 이름의 글자 수가 2글자 이하면 다음과 같이 처리한다.
    - check if user name contains under 2 characters


<br>

#### 2.2.4 띄어쓰기를 없애고, 두 번째 단어부터는 첫 글자를 대문자로 

1. (함수1) 사용자 이름을 input 태그에서 가져온다.
    - getUserNameFromInputField()
2. (함수2) 사용자 이름의 글자 수가 2글자 이하면 다음과 같이 처리한다.
    - checkIfUserNameContainsUnder2Characters()

<br>

#### 2.2.5 함수를 사용할 때 의미상 없어도 되는 단어를 없애

1. (함수1) 사용자 이름을 input 태그에서 가져온다.
    - getUserNameFromField()
2. (함수2) 사용자 이름의 글자 수가 2글자 이하면 다음과 같이 처리한다.
    - checkUserNameUnder2Characters()

***
<br>

## 참고
- 개발자의 글쓰기