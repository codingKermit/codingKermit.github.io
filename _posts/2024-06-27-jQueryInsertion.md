---
title: 제이쿼리 객체 추가 / 이동
date: 2024-06-27
categories:
  - javascript
  - jQuery
tags:
  - javascript
  - jQuery
---
안녕하세요 🐸

##### 문제의 발단...

이번에 업무 중에 잘 보이던 버튼이 갑자기 사라지는 문제가 발생했습니다!

하나의 화면에서 다수의 탭을 두고 레이아웃이 바뀌는 구조에서 하나의 버튼으로 모든 탭에서 사용하려고 하니 CSS가 꼬이는 문제였습니다.

CSS를 조정 하는 것은 다른 작업이 일어나 인접 요소의 CSS가 변경될 경우 같은 문제가 다시 발생할 수 있다 생각했습니다.

그래서 각 탭의 특정 요소의 아래에 버튼이 위치하도록 요소의 위치 자체를 바꾸는 방법에 대해 생각해봤습니다.

jQuery의 공식 홈페이지에서는 DOM에 삽입하는 방법을 크게 3가지로 분류했습니다.
1. [요소의 둘레에 삽입](#요소의-둘레에-삽입)
2. [요소의 내부에 삽입](#요소의-내부에-삽입)
3. [요소의 외부에 삽입](#요소의-외부에-삽입)

## 요소의 둘레에 삽입

여기서 사용되는 함수들은 요소를 둘러싸는 요소를 추가하는 함수입니다.

주로 사용되는 내부/외부에 삽입하는 기능과 다르게 이 부분은 했갈리는 부분이 있을 수 있기 때문에 예제 소스와 함께 설명하겠습니다.

### unwrap()
선택된 각각의 요소의 부모 요소를 삭제합니다.
### wrap()
선택된 각각의 요소에 부모 요소를 추가합니다.
<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/MeowMeowPuppy/embed/MWdPxOY?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/MeowMeowPuppy/pen/MWdPxOY">
  Untitled</a> by MeowMeowPuppy (<a href="https://codepen.io/MeowMeowPuppy">@MeowMeowPuppy</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

unwrap(), wrap()의 예시입니다.
### wrapAll()
선택된 요소 집합에 부모 요소를 추가합니다.

선택되는 요소의 집합이기 때문에 경우의 수가 많아 했갈리는 부분이 많이 있습니다.

#### 선택한 요소가 인접한 경우
<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/MeowMeowPuppy/embed/wvbQwBg?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/MeowMeowPuppy/pen/wvbQwBg">
  Untitled</a> by MeowMeowPuppy (<a href="https://codepen.io/MeowMeowPuppy">@MeowMeowPuppy</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

선택된 요소들이 인접한 경우에는 쉽게 예상할 수 있듯이 요소들이 하나의 부모 요소 아래에 위치하게 됩니다.

#### 사이에 선택되지 않은 요소가 껴있는 경우

<iframe height="300" style="width: 100%;" scrolling="no" title="jQuery wrapAll() sample3" src="https://codepen.io/MeowMeowPuppy/embed/PovxqzG?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/MeowMeowPuppy/pen/PovxqzG">
  jQuery wrapAll() sample3</a> by MeowMeowPuppy (<a href="https://codepen.io/MeowMeowPuppy">@MeowMeowPuppy</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

요소의 깊이가 다른 경우 혹은 사이에 선택되지 않은 요소가 선택된 경우에는 가장 첫 번째 요소를 기준으로 나머지 요소를 가지고 와서 부모 요소를 삽입합니다.  
경우의 수가 많기 때문에 이 경우에는 어떻게 되는지 궁금했는데 이렇게 되는군요

### wrapInner()
선택한 요소의 내부를 감싸는 요소를 삽입합니다.

<iframe height="300" style="width: 100%;" scrolling="no" title="jQuery wrapInner" src="https://codepen.io/MeowMeowPuppy/embed/NWVmYrR?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/MeowMeowPuppy/pen/NWVmYrR">
  jQuery wrapInner</a> by MeowMeowPuppy (<a href="https://codepen.io/MeowMeowPuppy">@MeowMeowPuppy</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

outer 클래스의 내부에 inner 클래스 요소가 생성되는 것을 확인할 수 있습니다.


## 요소의 내부에 삽입

### append()

요소의 내부에 객체를 삽입합니다.

아래의 형식을 가집니다.

```
$('ul').append('<li>Hi!</li>')
```

ul 태그 내부의 마지막에 li가 추가됩니다.

> ~에 ~를 추가한다
### appendTo()

`append()` 와 동일한 기능을 가지지만 형식이 다릅니다.

```
$('<li>Hello!</li>').appendTo('ul')
```

li를 ul의 마지막에 삽입합니다.

>~를 ~에 추가한다

### html()

파라미터 없이 사용할 경우 선택한 요소의 html을 반환합니다.

파라미터를 입력한다면 선택한 요소 내부의 html 코드를 변경합니다.

```
<div>
	<p>Hi!</p>
</div>

...생략

$('div').html(); // <p>Hi!</p> 를 반환
$('div').html("<p>Bye!</p>"); // div의 내부 html을 <p>Bye!</p> 로 변경

```

### prepend()

선택한 요소 내부의 첫 번째 위치에 요소를 삽입합니다.

```
$('ul').prepend('<li>One</li>')
```

ul 의 첫 번째 요소로 li가 추가됩니다.
### prependTo()

prpend()와 기능은 같습니다. 하지만 사용 방식은 appendTo()와 같습니다.

```
$('<li>First</li>').prependTo('ul');
```

li를 ul의 첫 번째에 삽입합니다.
### text()

선택한 요소의 내부에 텍스트를 삽입하거나 텍스트 값을 가져옵니다.

변수 없이 사용하면 내부의 html요소를 제외한 순수 텍스트만을 반환합니다.

내부에 값을 입력한다면 요소 내부를 입력한 값으로 대체합니다.

```
<div>
	<p>누구세요?</p>
</div>

...생략

$('p').text(); // 누구세요? 반환

$('p').text('뚱인데요?'); // p의 내부를 뚱인데요? 로 대체\


/* 
p의 내부를 <b>사랑해요</b> 로 대체. html 태그로 처리되지 않고 순수 텍스트의 형식으로 삽입
*/
$('p').text('<b>사랑해요</b>'); 


```

## 요소의 외부에 삽입

### after()

선택한 요소의 다음에 삽입합니다.

```
<p>금동이</p>

...생략

$('p').after('<p>은동이</p>') // 금동이 뒤에 삽입
```

### before()

선택한 요소의 앞에 삽입합니다

```

<p>가위</p>

...생략

$('p').before('<p>바위</p>') // 가위 앞에 삽입

```

### insertafter()

after()과 같은 기능을 합니다. 하지만 코드 순서가 다릅니다.

```
<p>고추장</p>

...생략

$('<p>된장</p>').insertAfter('p'); // 고추장 뒤에 된장 삽입

```

### insertBefore()

```
<p>김치찌개</p>

...생략

$('<p>된장찌개</p>').insertBefore('p'); // 김치찌개 앞에 된장찌개 삽입

```



했갈리는 함수들에 대해서는 아래의 표로 정리해볼 수 있겠습니다.

| A를 B에 삽입(이동) | B에 A를 삽입(이동) |
|:------------------:|:------------------:|
|      append()      |     appendTo()     |
|     prepend()      |    prependTo()     |
|      after()       |   insertAfter()    |
|      before()      |   insertBefore()   |



---
참고문서 : [https://api.jquery.com/category/manipulation/](https://api.jquery.com/category/manipulation/)
