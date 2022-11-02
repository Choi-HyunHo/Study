# React - Spread 사용

[자바스크립트 부분에서 한 번 공부를 했습니다.](../js/spread.md)
- 하지만 React 에서 불변성의 규칙을 지키기 위해 종종 사용되곤 해서 
- 리액트에서는 어떻게 사용되는지 정리 합니다.

***
<br>

## React 사용하는 방법은

리액트에서는 state 값을 변경할 때 불변성을 지켜야 합니다.
- 불변성이란 어떤 값을 직접 변경하면 안 된다는 의미
- 변경하는 대신 복사 등의 방법을 통해 새로운 값을 만들어야 합니다.

***
<br>

## 사용법
- useState 의 상태변화 함수 안에서 아래와 같이 사용 합니다.

***
<br>

## 예시
```js
import React, { useState } from 'react';

const AppMentor = () => {
  const [person, setPerson] = useState({
    name: '현호',
    title: '개발자',
    mentor: {
      name: '제임스',
      title: '시니어개발자',
    },
  });
  return (
    <div>
      <h1>
        {person.name}는 {person.title}
      </h1>
      <p>
        {person.name}의 멘토는 {person.mentor.name} ({person.mentor.title})
      </p>
      <button
        onClick={() => {
          const name = prompt(`what's your mentor's name?`);
          setPerson(({...person,  mentor : { ...person.mentor, name }}))
        }}
      >
        멘토 이름 바꾸기
      </button>
    </div>
  );
}

export default AppMentor;
```

```js
<button
      onClick={() => {
          const name = prompt(`what's your mentor's name?`);
          setPerson(({...person,  mentor : { ...person.mentor, name }}))
      }
}>
```

- 상태변화 함수 안에서 person 의 값을 그대로 복사하여 가지고 
- mentor 에는 또 객체가 있는데 {기존의 데이터 유지, 업데이트 할 항목} 이라는 의미 입니다.
- 즉, ...person 을 가져와서 그 다음에 사용되는 현재는 mentor 의 값으로 덮어씌웁니다.
- 뒤의 name 은 이름만 업데이트 한다는 의미 입니다.
