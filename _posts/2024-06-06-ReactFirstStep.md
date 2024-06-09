---
title: 리액트 맛보기 1일차
date: 2024-06-06
categories:
  - Front End
  - React
tags:
  - React
  - FrontEnd
---
리액트 공부 1일차

## 01. JSX?
- Javascript의 확장 언어
리액트에서 HTML 처럼 사용하는데 이 언어가 JSX.
우리가 JSX로 작성한 코드는 바벨을 통해 바닐라 자바스크립트로 변환되어 요소를 생성합니다.
JSX에서 

```
<h1>응애 나는 아기 h1 태그</h1>
```

이런 코드를 작성했으면 HTML로 바로 해당 코드가 쓰이는 것이 아니라 자바스크립트에서 위의 구조를 가진 요소를 만드는 코드가 실행되어 요소가 생성되는 것입니다.
결과적으로 위의 코드와 아래의 코드는 동일 합니다.

```
React.createElement("h1",null,"응애 나는 아기 h1 태그")
```

## 02. State
JSX에서는 스크립트에서 만든 변수를 {} 내에 입력하여 사용할 수 있습니다.
이때 예시로 아래와 같은 코드가 있을때

```
const title = useState('title');
...생략
<h1>{title}</h1>
```

title의 값을 바로 변경하면 h1 태그 내의 값이 변경 되지 않습니다.
`useState()` 의 반환 값은 배열로 전달되며 첫번째는 값, 두번째는 이 변수를 컨트롤 하는 함수로 전달 됩니다.

그래서 예시를 보자면 아래와 같은 형태를 가집니다.
```
const [title, setTitle] = useState('title');
```

여기서 `setTitle` 은 title 이라는 첫 번째 변수의 값은 컨트롤 하는 함수의 역할을 하며 괄호 내에 값을 입력하면 변수의 값이 변경되며 해당 값을 사용하는 요소가 다시 렌더링 됩니다.

**+ 여기서 궁금했던 점**
값이 배열일 때 변수도 배열로 입력해서 받는 것은 자바스크립트의 문법 중 하나 입니다.
이것을 **구조 분해 할당** 이라고 부릅니다.
그래서 `useState()` 는 2의 길이를 가지는 배열을 반환함을 알 수 있는데 그렇다면 아래의 코드도 정상 수행 되는게 아닐까? 하는 생각을 했습니다.

```
const data = useState('data');
const dataVal = data[0];
const setDataVal = data[1];
```

당연하지만 이거 됩니다(!!!). 근데 코드 줄만 늘어나고 비 효율적이니 깔끔하게 한 줄로 씁시다.


***특이사항 발견!***  
###### 예시 상황

```
const [data, setData] = useState();
```

위 처럼 `useState()` 의 괄호를 빈 값으로 넣었을 경우  `setData()` 를 통해 값을 변경하면 에러가 발생합니다.  

```
A component is changing an uncontrolled input to be controlled. This is likely caused by the value changing from undefined to a defined value, which should not happen. Decide between using a controlled or uncontrolled input element for the lifetime of the component.
```
 
이 에러는 `useState()` 를 빈값으로 호출 했을 경우 주어지는 변수의 값이 undefind가 되기 때문에 발생하는 에러 입니다. 앱 전체에 큰 영향을 끼치는 에러는 아닌 것으로 보이지만 이런 에러를 발생시키지 않게 하기 위해서 초기 값을 설정하는 습관을 들이는 것이 좋겠습니다.

## 03. className, htmlFor
위에서 설명했듯이 우리가 HTML처럼 쓰고는 있지만 실제로 HTML은 아니기 때문에 class, for 같은 키워드는 사용하면? 에러가 발생하겠죠?  
예시로 class는 class를 정의할 때 사용되는 예약어고 for도 이처럼 예약어로 사용되고 있는 키워드입니다.  
그래서 예약어로 사용되는 키워드는 살짝 다르게 쓰이고 있습니다. 이거 모르면 왜 안되지? 하면서 구글링만 합니다.  

## 04. value, onChange
input 태그에서 value와 onChange는 세트로 작성 해야 합니다.

```
const [time, setTime] = useState('');
...생략
<input value={time}/>
```

위와 같이 세팅해두었다면 일반적인 jsp, html에서는 사용자의 입력 값이 바로바로 `time` 에 반영이 되지만 리액트는 다릅니다.
위의 예시의 경우에는 onChange에서 `setTime()` 을 사용해 값을 변경 해줘야 합니다.
value만 사용한다면 사용자가 아무리 값을 입력해도 화면에는 반영되지 않습니다.

=> 수정된 예시

```
const [time, setTime] = useState('');
const onChange = (e) =>{
	setTime(e.target.value);
}
...생략
<input value={time} onChange={onChange}/>
```

**+ value만 사용하면 발생하는 에러**  

```
You provided a `value` prop to a form field without an `onChange` handler. This will render a read-only field. If the field should be mutable use `defaultValue`. Otherwise, set either `onChange` or `readOnly`.
```

***발견한 특이사항***  
위에서 사용한 예시 코드에서 `useState()` 에 초기 값을 `''` 으로 설정했습니다. 하지만
`useState()`를 통해 변수를 생성했을 때에 괄호 내에 아무런 값을 입력하지 않은 경우에는 onChange를 함께 사용하지 않아도 화면에는 값이 변경되는 것이 확인 됩니다. 하지만 실제로 value로 설정한 변수에 사용자의 입력 값이 저장되고 있는 것은 아닙니다.
그리고 위에서 얘기했듯 `useState()`를 사용할 때에는 초기 값을 설정 하는 것이 더 좋아 보입니다.  
리액트...재밌네?
![](/assets/img/kermit/lovelyKermit.jpg)