---
title: 리액트 맛보기 3일차
date: 2024-06-18
categories:
  - React
tags:
  - 리액트
  - 프론트엔드
  - FrontEnd
  - React
---
안녕하세요 리액트 맛보기 3일차 입니다!

## 01. useEffect()

지난 시간에는 `useEffect()` 를 활용하여 컴포넌트가 생성되는 최초 시점에 1회만 동작하는 함수를 만들 수 있다고 배웠습니다.

덧붙여 설명하자면 `useEffect()` 컴포넌트의 라이프사이클을 추적하여 이의 변화를 감지하고 함수를 수행합니다.

그렇기 때문에 아래의 예제에서 `<Hi/>`는 `show`의 값에 따라 마운트되고 언마운트되기 때문에 `useEffect()` 내의 코드는 컴포넌트가 마운트될 때 마다 수행됩니다.

```jsx

function Hi(){
	useEffect(()=>{
		console.log("run once");
	},[])
	return <h1>Hi!</h1>
}

function App(){
const [show,setShow] = useState(true);
const onClick = () => setShow((prev) => !prev);
	return(
		<div>
			{show ? <Hi/> : null}
			<button onClick={onClick}>{show ? "Show" : "Hide"}</button
		</div>
	)
}
```

## 02. Clean-up function
클린업 함수는 useEffect()에서 컴포넌트가 언마운트될 때를 감지하여 동작하는 함수입니다.

```jsx

function Hi(){
	useEffect(()=>{
		console.log("created");
		return () => console.log("destroyed")
	},[])
	return <h1>Hi!</h1>
}

function App(){
const [show,setShow] = useState(true);
const onClick = () => setShow((prev) => !prev);
	return(
		<div>
			{show ? <Hi/> : null}
			<button onClick={onClick}>{show ? "Show" : "Hide"}</button
		</div>
	)
}
```

위 예제의 경우 버튼을 클릭하여 `<Hi/>` 컴포넌트가 마운트 될 때 `created` 로그가 남고 언마운트 될 때 `destroyed` 로그가 남습니다. `useEffect()` 의 첫 번째 인자인 함수에서 `return` 을 명시함으로써 컴포넌트가 언마운트 될 때 수행할 동작을 정할 수 있습니다.  

지금까지 배운 것을 정리하여 `useEffect()`의 구조를 다시 본다면 아래와 같다 할 수 있겠습니다.  

```jsx

useEffect(()=>{
	// 컴포넌트가 마운트될 때 or 추적할 변수의 값이 변경될 때 동작할 내용

	return () => {
		// 컴포넌트가 언마운트 될 때 동작할 내용
	}
},[/* 추적할 변수 */])
```

`useEffect()` 는 단순히 추적할 변수의 변화를 감지하는 기능만을 하는 것이 아니라 컴포넌트의 라이프사이클 훅을 감지하고 동작을 수행하도록 하는 함수라 할 수 있겠습니다!


## 03. 라이프사이클

`useEffect()` 에 대해 공부하면서 리액트의 라이프사이클에 대해 궁금해져서 찾아봤습니다

라이프 사이클은 크게 3가지로 나눌 수 있습니다
1. 마운트
2. 업데이트
3. 언마운트

`useEffect()` 에서 두 번째 인자의 변수들을 추적 하여 컴포넌트를 업데이트합니다.  
그렇기 때문에 `useEffect()` 에서 변수의 추적은 컴포넌트를 업데이트를 하기 위함이고 컴포넌트의 라이프 사이클을 추적한다는 것이 보다 옳은 표현이라 생각합니다. 