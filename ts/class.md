# 타입스크립트의 클래스
## 자바스크팁트의 클래스
- ES2015 (ES6) 부터 생겼습니다.
```js
// 기존의 생성자 함수
function Person(name, age){
    this.name = name;
    this.age = age;
}

var seho = new Person('세호', 30)

// 클래스
class Person{
    // 클래스 로직
    constructor(name, age){
        console.log('생성 되었습니다');
        
        this.name = name;
        this.age = age;
    }

}

var seho = new Person('세호', 30); 
console.log(seho)
```
- 위의 두 개의 코드는 같습니다.

<br>

## 타입스크립트의 클래스
- 문법적인 차이가 있습니다.
```js
class Person {
    name: string; // class 안에서만 사용하고 싶으면 앞에 private 사용
    age: number; // 기본적으로 public
    readonly log: string // 읽기전용
 

    constructor(name: string, age: number){
        this.name = name;
        this.age = age;
    }
}

```

<br>

## 읽기 전용
- 클래스 속성에 `readonly` 키워드를 사용하면 아래와 같이 접근만 가능합니다.
```js
class Developer {
    readonly name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}
let john = new Developer("John");
john.name = "John"; // error! name is readonly.
```

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8/dashboard)
- [캡틴판교](https://joshua1988.github.io/ts/guide/classes.html#readonly)