# 타입 가드(Type Guard)
특정 타입으로 타입의 범위를 좁혀나가는(필터링 하는) 과정

<br>

## 설명하기 위한 사전 예제
```js
interface Developer{
    name: string;
    skill: string;
}

interface Person {
    name: string;
    age: number;
}

function introduce(): Developer | Person{
    return {name: 'Tony', age: 33, skill:"Iron Making"}
}

let tony = introduce();
console.log(tony.skill); // 유니온 특징에 의해 접근 불가

// 타입 단언을 사용하여 접근
if((tony as Developer).skill){
    let skill = (tony as Developer).skill;
    console.log(skill);
} else if(tony as Person).age){
    let age = (tony as Developer).age;
    console.log(age);
}
```
- 가독성이 많이 떨어집니다.

<br>

## 타입 가드를 적용하면
- `is` 키워드 사용

```js
function isDeveloper(target: Developer | Person): target is Developer {
    return (target as Developer).skill !== undefined; // target이 skill 이라는 것이 있을 경우, Developer 취급 한다.
}

if(isDeveloper(tony)){
    tony.skill
} else{
    tony.age
}
```

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)