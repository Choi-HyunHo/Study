#  JSON (JavaScript Object Notation)

데이터를 저장하거나 전송할 때 많이 쓰이는 경량의 데이터 교환 표준 

<br>

## 특징
- 서버와 클라이언트 간의 교류에서 일반적으로 많이 사용 합니다.
- JSON 형식에서는 `null, number, string, array, object, boolean` 을 사용할 수 있습니다.

- JSON 형식은 자바스크립트 객체와 마찬가지로 `key/value` 형식으로 구성되며, key값이나 문자열은 항상 쌍따옴표("")를 이용하여 표기 합니다.

<br>

## JSON.stringify()
- __객체를 JSON으로 바꿔줍니다.__

```js
let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

let json = JSON.stringify(student);

alert(typeof json); // 문자열이네요!

alert(json);
/* JSON으로 인코딩된 객체:
{
  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}
*/
```
<br>

- JSON.stringify(student)를 호출하자 student가 문자열로 바뀌었습니다.

- 이렇게 변경된 문자열은 JSON으로 인코딩된(JSON-encoded), 직렬화 처리된(serialized), 문자열로 변환된(stringified), 결집된(marshalled) 객체라고 부릅니다.

- __객체는 이렇게 문자열로 변환된 후에야 비로소 네트워크를 통해 전송하거나 저장소에 저장할 수 있습니다.__

<br>

## JSON.stringify 호출 시 무시되는 프로퍼티

1. 함수 프로퍼티 (메서드)
2. 심볼형 프로퍼티 (키가 심볼인 프로퍼티)

3. 값이 undefined인 프로퍼티

<br>

## JSON.stringify 속성
```js
let json = JSON.stringify(value[, replacer, space])
```
- `value` : 인코딩 하려는 값
- `replacer` : JSON으로 인코딩 하길 원하는 프로퍼티가 담긴 배열. 또는 매핑 함수 function(key, value)

- `space` : 서식 변경 목적으로 사용할 공백 문자 수

<br>

### replacer
- `replacer` 인자에 배열을 주면, 배열값이 허용할 프로퍼티로 해석되어 추가합니다. __즉, 이것만 반환하라! 뜻입니다.__

- 만약 replacer 값 이 없거나 null 이면, 모든 값이 반환됩니다.

```js
// 두번 째 인자로 배열을 넣어주면, 그 key값만 참조해서 반환한다.
json = JSON.stringify(rabbit, ['name', 'size']);
// {"name":"tori","size":null}
```

<br>

### space
- 가독성을 높이기 위해 중간에 삽입해 줄 공백 문자 수를 나타내는 인자입니다.
```js
let user = {
  name: "John",
  age: 25,
  roles: {
    isAdmin: false,
    isEditor: true
  }
};

alert(JSON.stringify(user, null, 2));
/* 공백 문자 두 개를 사용하여 들여쓰기함:
{
  "name": "John",
  "age": 25,
  "roles": {
    "isAdmin": false,
    "isEditor": true
  }
}
*/

/* JSON.stringify(user, null, 4)라면 아래와 같이 좀 더 들여써집니다.
{
    "name": "John",
    "age": 25,
    "roles": {
        "isAdmin": false,
        "isEditor": true
    }
}
*/
```

<br>

## JSON.parse()
-  JSON을 객체로 바꿔줍니다.
```js
let value = JSON.parse(str, [reviver]);
```
- `str` : JSON 형식의 문자열

- `reviver` : 모든 (key, value) 쌍을 대상으로 호출되는 function(key,value) 형태의 함수로 값을 변경시킬 수 있습니다.
```js
// 문자열로 변환된 배열
let numbers = "[0, 1, 2, 3]";

numbers = JSON.parse(numbers);

alert( numbers[1] ); // 1


// 중첩 객체
let userData = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

let user = JSON.parse(userData);

alert( user.friends[1] ); // 1
```

<br>


### reviver
- 함수명에서 유추할 수 있듯이, 객체로 변환하면 자체 내장함수나 심볼 등을 사용 할 수 있으니, 이를 복구, 소생 시킨다는 기능을 가지고 있습니다.

```js
let json = JSON.stringify(true);
console.log(json)

json = JSON.stringify(['apple','banana'])
console.log(json)

const rabbit = {
    name : 'coco',
    color : 'white',
    size : null,
    birthDate : new Date(),
    jump : ()=>{
        console.log(`${name} can jump`)
    },
};

json = JSON.stringify(rabbit);
console.log(json);

json = JSON.stringify(rabbit, ["name"]); // 내가 원하는 속성만 골라서 JSON으로 변환 가능
console.log(json);

json = JSON.stringify(rabbit, (key, value)=>{
    // callback 사용 가능
});

// 1번째
json = JSON.stringify(rabbit)
const obj = JSON.parse(json)
console.log(obj)
rabbit.jump() // 사용이 가능한 이유는 JSON으로 변환 할 때 함수가 포함되어 있지 않았기 때문
obj.jump() // 에러 발생

// 2번째, reviver
json = JSON.stringify(rabbit)
const obj = JSON.parse(json, (key, value)=>{
    return key === 'birthDate' ? new Date(value) : value;
})
console.log(obj)
rabbit.jump() // 사용이 가능한 이유는 JSON으로 변환 할 때 함수가 포함되어 있지 않았기 때문
//obj.jump() // 에러 발생

console.log(rabbit.birthDate.getDate())
console.log(obj.birthDate.getDate()) // 에러없이 출력 가능
```

<br>

## 정리
- `JSON.stringify` 를 사용하면 원하는 값을 JSON으로 직렬화 할 수 있고

- `JSON.parse` 를 사용하면 JSON을 본래 값으로 역 직렬화 할 수 있습니다.

<br>

## 참고
- [MDN, JSON](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)
- https://ko.javascript.info/json
- [자바스크립트로 json 다루기](https://inpa.tistory.com/entry/JSON-%F0%9F%93%91-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A1%9C-JSON-%EB%8B%A4%EB%A3%A8%EA%B8%B0)
- [코드 안식처](https://blog.naver.com/x7788/222651945145)
- [드림코딩](https://www.youtube.com/watch?v=FN_D4Ihs3LE&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=10)