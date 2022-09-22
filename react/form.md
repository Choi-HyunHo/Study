# React - Form
폼은 양식이라는 뜻을 가지고 있습니다.<br>
그 중 사용하는 방법이 비슷하고 자주 사용하는 태그 3개에 대해서 정리 하였습니다.

<br>

> 즉, 사용자로부터 입력을 받기 위해 사용하는 것 입니다.

<br>

## input 태그
```js
const NameForm = () => {
    const [value, setValue] = useState('');

    const handleChange = (e) => {
        setValue(e.target.value);
    }

    const handleSubmit = (e) => {
        alert('입력한 이름 : ', value);
        e.prevenDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                이름 : 
                <input type='text' value={value} onChange={handleChange}/>
            </label>
            <button type='submit'>제출</button>
        </form>
    )
}
```

<br>

`<input>` 태그의 value={value} 부분은 리액트 컴포넌트의 __state__ 값을 가져다 넣어야 합니다.<br> 그래서 항상 __state__ 에 들어있는 값이 input 에 표시 됩니다.

<br>

또한 입력 값이 변경 되었을 때 호출되는 onChange 에는 onChange={handleChange} 처럼 handleChange 함수가 호출되도록 했는데,<br> handleChange 함수에는 __setValue()__ 함수를 사용하여 새롭게 변경된 값을 __value__ 라는 이름의 __state__ 에 저장 합니다.

<br>

참고로 onChange 의 첫 번째 파라미터인 e 는 이벤트 객체를 나타냅니다. <br>
그리고 __e.target__ 은 현재 발생한 이벤트의 타겟을 의미하며, <br>
__e.target.value__ 는 해당 타겟의 __value__ 속성 값을 의미 합니다.

<br>

> 즉, 여기서의 타겟은 input 이 되며, e.target.value 는 input 의 값이 됩니다.

<br>

위와 같은 방식으로 Form 을 사용 합니다.

***
<br>

## textarea 태그
`<textarea>` 태그는 여러 줄에 걸쳐서 나올 정도로 긴 텍스트를 입력받기 위한 태그 입니다. <br>
사용하는 방법은 위의 input 과 크게 다르지 않습니다.

<br>

```js
const RequestForm = () => {
    const [value, setValue] = useState();

    const handleChange = (e) => {
        setValue(e.target.value);
    }

    const handleSubmit = (e) => {
        alert('입력한 요청사항 : ', value);
        e.prevenDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                요청사항 : 
                <textarea value={value} onChange={handleChange}/>
            </label>
            <button type='submit'>제출</button>
        </form>
    )
}
```

***
<br>

## select 태그
`<select>` 태그는 여러 가지 옵션 중 하나를 선택하는 기능을 제공하는 태그 입니다.
- `<select>` 태그가 `<option>` 태그를 감싸는 형태로 사용 됩니다. 

<br>

```js
const FruitSelect = () => {
    const [value, setValue] = useState('banana');

    const handleChange = (e) => {
        setValue(e.target.value);
    }

    const handleSubmit = (e) => {
        alert('선택한 과일 : ', value);
        e.prevenDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                과일 선택 : 
                <select value={value} onChange={handleChange}>
                    <option value='apple'>사과</option>
                    <option value='banana'>바나나</option>
                    <option value='watermelon'>수박</option>
                    <option value='grape'>포도</option>
                </select>
            </label>
            <button type='submit'>제출</button>
        </form>
    )
}
```

***
<br>

## 참고
- 소풀의 처음 만난 리액트