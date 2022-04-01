# Enum ( 이넘 )

특정 값들의 집합을 의미하는 자료형

<br>

## 숫자형 이넘
```js
enum Shoes{
    Nike, // 0
    Adidas // 1
}

let myShoes = Shoes.Nike;
console.log(myShoes); // 0


```
- 별도의 값을 지정하지 않으면 숫자형으로 취급 합니다.

<br>

```js
enum Direction {
  Up = 1,
  Down,
  Left,
  Right
}

// 결과
Up - 1
Down - 2
Left - 3
Right - 4
```

<br>

## 문자형 이넘
```js
enum Shoes{
    Nike = '나이키',
    Adidas = '아디다스'
}

let myShoes = Shoes.Nike;
console.log(myShoes);// '나이키' 
```

<br>

## 예시
### 사용하기 전
```js
function askQuestion(answer: string){
    if(answer === 'yes'){
        console.log('정답입니다');
    }

    if(answer === 'no'){
        console.log('오답입니다');
    }
}

askQuestion('yes'); 
askQuestion('예스'); 
askQuestion('y'); 
```
- 파라미터를 문자열로 설정 했기 때문에 'yes' 라는 의미를 가진 문자열을 넣을 수 있습니다.

<br>

### enum 사용
```js
enum Answer {
    Yes = 'Y',
    No = 'N'
}

// answer 의 타입을 string -> Answer 이넘으로 변경
function askQuestion(answer: Answer){
    if(answer === Answer.Yes){
        console.log('정답입니다');
    }

    if(answer === Answer.No){
        console.log('오답입니다');
    }
}

askQuestion(Answer.Yes);
askQuestion('Yes'); // 에러 발생, 이넘을 이용해서 정의하면 이넘에서 제공하는 데이터만 사용 가능 
```
- 목록이 필요한 형태에서 `enum` 을 정의해서 사용하는 것이 정확한 코드, 예외 처리의 케이스를 줄일 수 있습니다.
