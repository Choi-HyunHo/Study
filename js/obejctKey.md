# JavaScript Object[‘key’] vs Object.key 차이

해당 글의 원본은 [JavaScript Object[‘key’] vs Object.key 차이](https://medium.com/sjk5766/javascript-object-key-vs-object-key-%EC%B0%A8%EC%9D%B4-3c21eb49b763) 입니다.

<br>

JavaScript 객체의 property를 접근 하는 방법에는 []와 . 을 사용하는 방법이 있습니다. 

```js
var a = {
 b : 1,
 c : 2
}
console.log(a["b"] + ' vs ' + a.b) // 1 vs 1
```

<br>

## 설명
```js
var a = {
 “a” : 1,
 “b-c”: 2,
 “0d” : 3,
 “d0” : 4
}
console.log(a.a)   // 1
console.log(a.b-c) // ReferenceError: c is not defined
//console.log(a.0d)// SyntaxError: Invalid or unexpected token
console.log(a.d0)  // 4
```

- 두 번째 console.log(a.b-c) 에서 c가 선언되지 않았다는 에러가 발생합니다. 
- a.b-c에서 어떻게 해석되냐면 객체 a의 속성 b와 변수 c를 뺴라는 연산으로 인지됩니다.
-  . 표현으로 접근할 수 없고 a[“b-c”]로 표현되어야 합니다.
- 주석 처리한 세 번째 console.log는 주석을 풀고 실행하면 에러가 발생합니다. 
- __왜냐면 . 표현에서 숫자가 먼저 나오면 에러가 발생합니다.__ ( 아래 참고 )

```js
var a = {
 a : 1,
 2 : 2,
 d3 : 3
}
console.log(a.a)
console.log(a[‘2’])
console.log(a.2) // error 발생
```

- 객체 a는 property로 2를 가집니다. 
- 2에 대한 접근은 a[‘2’]로는 가능하지만 a.2는 에러가 발생합니다. 
- 마찬가지로 a[‘2a’]는 가능하지만 a.2a는 에러가 발생합니다.

> 반면에 [] 으로 객체에 속성값이 문자열이라면 전부 접근가능합니다.

<br>

## [ ] 표현은 변수로 접근 가능하지만 . 표현은 바로 객체의 속성에 접근합니다. 
```js
var a = {
 b : 1,
 c : 2
}
var b = ‘c’
console.log(a[b] + ‘ vs ‘ + a.b) // 2 vs 1
```

- console.log 에서 a[b]와 a.b로 접근 했습니다. 헌데 b는 변수로 c라는 값을 가지고 있습니다. 
- a[b]에서는 b가 변수가 되어 실제로 객체 a의 속성 c의 값인 2를 출력하는 반면,
- a.b에서는 b가 변수가 아닌 실제 속성 b에 접근하여 1을 출력하게 됩니다.

<br>

## 예시
```js
var a = {
  b  : 1,
  c  : 2
}
for (key in a) {
 console.log(a.key) // undefined, undefined
 console.log(a[key])// 1,2
}
```
- for문과 같이 객체에 속성 개수만큼 loop를 돌 떄 변수를 사용하는데 a[key]를 사용하면 key는 실제 속성값으로 변경되지만, 
- a.key는 객체 a의 속성 key라는 문자열을 찾기에 undefined가 됩니다.
