---
title: 리액트 맛보기 2일차
date: 2024-06-16
categories:
  - 공부일기
  - React
tags:
  - FrontEnd
  - React
  - 프론트엔드
  - 리액트
---
리액트 맛보기 2일차!

오늘의 공부 내용을 정리합니다.

## 01. prop
부모 컴포넌트가 자식 컴포넌트에게로 전달하는 값
이 prop을 통해 자식 컴포넌트를 상황에 맞게 커스터마이징 하여 사용할 수 있도록 설계할 수 있습니다.

ex)
```jsx

const FirstBtn = (props) =>{
	<button>{props.text}</button>
}

const SecondBtn = ({text}) => {
	<button>{text}</button>
}

function App(){
	return(
		<FirstBtn text="This is First Button">
		<SeconBtn text="This is Second Button"
	)
}
```

위의 예시에서 사용한  예시로 `FirstBtn` 와 `SecondBtn` 컴포넌트가 있는데 두 번째 예시의 방법으
로 사용하는 것이 어떠한 prop을 사용할 것인지 명시적으로 사용하기 때문에 더 좋다고 생각합니다.  
두 번째 방법은 ES6의 문법을 응용한 방법입니다

###  02. memo
부모 컴포넌트에서 `<Btn/>` 이라는 자식 컴포넌트를 사용한다 가정할 때에, state에 따라 Btn을 다시 랜더링 하는 경우가 있습니다.
 이 때 특정한 하나의 `<Btn/>` 컴포넌트의 prop이 바뀌어 다시 랜더링 되는 경우 리액트에서는 모든 `<Btn/>` 컴포넌트를 다시 랜더링합니다.
 이렇게 된다면 화면 내에 `<Btn/>` 이라는 컴포넌트가 500개가 있다 가정하면 1개의 `<Btn/>` 컴포넌트를 다시 랜더링 하기 위해 500개의 컴포넌트를 랜더링 해야하는 문제가 발생하고 이는 속도를 느게 만드는 원인이 될 수 있습니다.
 이 것을 막기위해 있는 방법이 `memo()` 입니다. prop의 변화가 있지 않다면 랜더링을 하지 않도록 명시하는 기능을 합니다.
#### 개선 전
```jsx

const Btn = ({index}) => {
	return (
		<button>{index}</button>
	)
}

const [index, setIndex] = useState(0);

const changeIndex = () => setIndex(900)

function App(){
	return(
		{/* prop의 변화가 생기는 하나의 컴포넌트*/}
		<Btn index={index}/>
		
		{/* prop의 변화가 생기지 않는 다수의 컴포넌트들 */}
		<Btn index={1}/>
		...생략
		<Btn index={500}/>
	)
}
```
위의 코드는 prop이 변하는 하나의 컴포넌트를 랜더링 하기 위해 아래의 500개의 컴포넌트도 랜더링 됩니다.

#### 개선 후
```jsx

const Btn = ({index}) => {
	return (
		<button>{index}</button>
	)
}

const [index, setIndex] = useState(0);

const changeIndex = () => setIndex(900)

const MemoriezedBtn = memo(Btn);

function App(){
	return(
		{/* prop의 변화가 생기는 하나의 컴포넌트*/}
		<MemoriezedBtn index={index}/>
		
		{/* prop의 변화가 생기지 않는 다수의 컴포넌트들 */}
		<MemoriezedBtn index={1}/>
		...생략
		<MemoriezedBtn index={500}/>
	)
}
```
위의 코드는 prop이 변하는 첫 번째 컴포넌트를 제외한 나머지 컴포넌트는 다시 랜더링 되지 않습니다.
 
### 03. prop-Types
순수 자바스크립트에서 변수의 타입을 알 수 없듯이 리액트의 prop도 동일합니다.
어떤 컴포넌트의 개발자가 prop들을 어떤 타입으로 받기를 원하는지는 알 수 없습니다.
이 부분을 돕기 위한 라이브러리가 prop-types 입니다.
prop-types 라이브러리를 사용하여 prop의 타입을 명시하고 컴포넌트를 사용하는 개발자들에게 도움을 줄 수 있습니다.
```jsx
/***
const Btn = ({text, fontSize}) =>{
	return(
		<button 
			style={{
				fontSize : fontSize,
			}}
		>{text}</button>
	)
}

Btn.propTypes = {
	text : string,
	fontSize : number
}

function App(){
	return(
		<Btn text="This is String" fontSize={24}/>
		<Btn text="This is String" fontSize={"This is String"}/>
	)
}
*/
```
위와 같은 코드는 컴파일 에러를 발생 시키지 않지만 개발자도구에서 에러를 발생시킵니다.

=> 에러 예시
> Failed prop type: Invalid prop `fontSize` of type `string` supplied to `Btn`, expected `number`.

Prop-Types 에서 사용되는 타입들은 공식 홈페이지에서 확인할 수 있습니다.

[https://legacy.reactjs.org/docs/typechecking-with-proptypes.html](https://legacy.reactjs.org/docs/typechecking-with-proptypes.html)


### 04. create-react-app
리액트 프로젝트를 생성하는데 도움을 주는 라이브러리입니다.
처음 제가 Vue.js를 공부할 때에도 이와 같은 기능을 하는 라이브러리가 있었는데 리액트에도 있군요!
명령문은 간단합니다.
```
npx create-react-app my-name
```


### 05. CSS module
먼저 CSS module이 뭔지 알아야 합니다!
CSS module는 css 클래스를 불러와서 사용할 때 class 이름을 고유한 값으로 자동 생성해주는 모듈입니다.
개발하다보면 class 이름이 중첩되는 경우가 있는데 이렇게 되면 CSS 가 꼬이는 경우가 발생할 수 있습니다.
CSS module는 이것을 방지하여 개발자들이 손쉽게 개발할 수 있도록 돕습니다.

#### CSS module 의 생성 방법
CSS module를 사용하는 방법은 간단합니다.  

> [모듈명].module.css

위의 형식으로 파일명을 만들어 사용합니다. 가령 Apple.js 를 위한 CSS module을 만들고자 한다면 Apple.module.css 라는 이름으로 지어서 사용하면 됩니다.
여기서 파일명은 반드시! 이렇게 해야하는 것은 아니지만 이것은 룰입니다. 모두가 그렇게 하기로 한 약속이지요.

#### CSS module 사용하기

아래는 CSS module을 사용하는 방법의 예제 코드입니다.
그리고 module화 되지 않은 css 파일을 module 처럼 사용했을 때에는 어떻게 되는지에 대한 예시도 함께 있습니다.

App.js 코드 예제   
````jsx

import Apple from "./Apple";

function App() {
	return (
		<div className="App">
			<Apple/>
		</div>
	);
}
export default App;
````


Apple.js 코드 예제   
````jsx

import cssModule from "./Apple.module.css";
import vanilaCss from "./Apple.css";

function Apple(){
	return(
		<div>
			<h1 className={cssModule.apple}>Hi! I am Apple with CSS module!</h1>
			<h1 className={vanilaCss.apple}>Hi! I am Apple with vanila CSS!</h1>
		</div>
	)
}

export default Apple;
````

Apple.module.css 와 Apple.css 의 구성은 동일합니다.   

````css
.apple{
	color:red;
}
````


````css
.apple{
	color:red;
}
````

1. CSS module 내에서 사용하고자 하는 class 이름에 대한 css를 작성합니다.
2. 사용할 위치에서 모듈을 import합니다.
3. 스타일을 적용하고자 하는 컴포넌트의 className에 모듈에 작성한 이름을 예제와 같은 형식으로 입력합니다.

적용 결과입니다
![](/assets/img/screenshot/2024-06-16-React/img1.png)

모듈화 되지 않은 CSS 파일을 사용했을 때에는 정상적으로 적용되지 않은 것을 확인할 수 있습니다.

그리고 여기서 눈여겨볼 것은 제가 class 이름을 apple로 만들었지만 html에서는 난수화된 class 이름이 적용되어 있는 것을 확인할 수 있습니다.

덕분에 개발자는 class 이름의 중복을 걱정하지 않고 개발에 집중할 수 있습니다!


### 06. useEffect()

리액트의 useEffect()는 특정 값의 변화가 일어날 때만 특정 동작이 수행되도록 명시하는 것입니다.

첫 번째 인자로 수행할 동작을 받고 두 번째 인자로 감지할 값을 받습니다.

예로 fruits 이라는 값이 있다고 가정할 때의 예제입니다.   

```jsx
function App(){
	const [fruits, setFruits] = useState('');

	const onChange = (event) => setFruites(event.target.value);

	useEffect(()=>{
		console.log('Fruits changed!')
	},[fruits]);

	return(
		<input 
			value={fruits}
			onChange={onChange}
			type="text"
			placeholder="write here!"
		/>	
	)
}

```

useEffect의 첫 번째 인자로 log를 찍도록 했고 두 번째 인자로 fruits를 줬습니다.

사용자가 값을 입력할 때마다 fruits의 값은 변화될 것이고 이때마다 로그가 찍힙니다

이를 응용하여 두 번째 인자를 빈 배열로 전달할 경우에는 화면이 랜더링 되는 최초 1회에만 수행되는 코드를 작성할 수 있습니다.

리액트 재밌따  
![Lichtenstein](https://images.weserv.nl/?url=https://codingkermit.github.io/assets/img/kermit/lovelyKermit.jpg&w=600&h=400&output=jpg&q=100)