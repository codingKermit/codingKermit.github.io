---
title: padStart()와 padEnd()
date: 2025-08-07 13:22
categories:
  - javascript
tags:
  - javascript
_sort:
---
안녕하세요 🐸  

업무에서 월(Month) 값을 다루는 일이 많은데요 이 값을 다룰 때 자주 쓰게 되는 함수입니다.  

적절하게 사용한다면 훨씬 다양하게 응용 할 수도 있는 함수이기도 합니다.  

분명 자주 쓰는 함수인데 쓸 때 마다 자꾸 깜빡하니 잘 기억하기 위해서 메모해봅니다 ╰（‵□′）╯  

## padStart()

padStart() 메서드는 String 값의 메서드로, 결과 문자열이 주어진 길이에 도달할 때까지 이 문자열의 시작 부분에 다른 문자열을 (필요하다면 여러 번) 채웁니다

string 값의 메서드이기 때문에 number 타입에서는 타입 에러가 발생합니다.  


```javascript
const january = '1';
const february = 2;


console.log(january.padStart(2,'0'))
// '01' 출력

console.log(february.padStart(2,'0'))
// TypeError : february.padStart is not a function

```

따라서 문자열로 형변환 하여 사용하는 함수인 `String()` 과 함께 해서 아래와 같이 사용한다면 보다 범용성 있게 사용할 수 있습니다.  

```javascript

function example(param){
	return String(param).padStart(2,'0');
}

const january = 1;

console.log(example(january));
// '01' 출력

```

---
## padEnd()
**`padEnd()`** 메서드는 이 문자열을 주어진 문자열(필요한 경우 반복됨)로 채워서 결과 문자열이 지정된 길이에 도달하도록 합니다

```javascript
const january = '1';
const february = 2;


console.log(january.padEnd(2,'0'))
// '10' 출력

console.log(february.padEnd(2,'0'))
// TypeError : february.padStart is not a function
```

`padStart()` 함수와 마찬가지로 `String()` 을 조합하여 보다 범용성 있게 사용할 수 있습니다.  

```javascript

function example(param){
	return String(param).padEnd(2,'0');
}

const january = 1;

console.log(example(january));
// '10' 출력

```

---
## padStart(), padEnd() 공통점


패딩 후 결과의 길이가 원본값 보다 짧다면 원본 문자열 그대로 반환합니다.  

```javascript
const hello = 'hello';
const world = 'world';

console.log(hello.padStart(3,'*'));
// 3은 'hello' 보다 짧기 때문에 'hello' 출력 

console.log(world.padEnd(3,'*'));
// 3은 'world' 보다 짧기 때문에 'world' 출력
```
<br>
패딩으로 채우는데 사용할 문자열이 패딩 후의 길이에 딱 맞지 않다면 길이에 맞게 채워지고 나머지는 잘립니다.  

```javascript
const str1 = "test";

console.log(str1.padEnd(10, "123456789"));
// 'test123456' 까지만 출력

console.log(str1.padStart(10, "123456789"));
// '123456test' 까지만 출력
```



---
참고 : <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padStart>
참고 : <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd>