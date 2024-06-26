---
title: 제네릭 공식문서 파해치기 (1)
date: 2024-06-20
categories:
  - Java
tags:
  - BackEnd
  - Java
  - 제네릭
  - Generic
---
안녕하세요 🐸

현재 파견 나온 서비스의 코드를 분석하면서 제네릭이라는 개념에 대해 궁금해졌습니다.

우리가 아주 많이 사용하는 List, Map 등에서 꾸준히 사용되고 있지만 정확히 무엇인지는 몰랐던 그 제네릭 에 대해 공부해보고 정리해봅니다.  
저는 자바 8버전을 기반으로한 자바 공식 문서를 참고했습니다!

### 01. 제네릭의 개발 배경?
제네릭이 무엇인지를 알기 전에 왜 제네릭이 나왔는지에 대해 알아보겠습니다.

>Generics add stability to your code by making more of your bugs detectable at compile time
>(제네릭은 컴파일 타임에 더 많은 버그를 발견하게 하여 코드에 안정성을 더합니다.)

언제나 새로운 기술이 그렇듯 제네릭은 보다 안전하고 보다 나은 코드 개발을 위해 나왔습니다.
컴파일 타임에 발견되는 오류는 쉽게 파악하고 수정할 수 있지만 런타임 에러는 이것이 어렵기 때문에 이를 보완하고자 해서 나왔습니다.
자바 1.5에서 처음 도입되었습니다.

## 02. 제네릭 전후 비교

가장 보편적으로 쓰이고 체감이 쉬운 제네릭의 사용 예시는 List가 있습니다. 제네릭이 나오기 전에는 List는 Object를 담는 객체였습니다.  
그래서 가장 쉬운 예시를 보자면 List 객체의 선언 자체가 달라졌습니다.

제네릭 적용 전
```java

List list = new ArrayList();
```
이를 원시타입이라고도 부릅니다. 자세한 부분은 후술하겠습니다  
[=> 04. 원시 타입](#04-원시-타입raw-type)  

<br>
제네릭 적용 후
```java

List<String> list = new ArrayList<>();
```

그리고 위에서 언급했듯 제네릭이 없던 때의 List는 Object를 담는 객체이기 때문에 항상 형변환을 해야했습니다.

제네릭 적용 전
```java

List list = new ArrayList();
list.add("Hello :)");
list.add("Bye Bye :(");
String hello = (String) list.get(0);
String bye = (String) list.get(1);
```

제네릭 적용 후
```java

List<String> list = new ArrayList<>();
list.add("Hello :) ");
list.add("Bye :( ");
String hello = list.get(0);
String bye = list.get(1);
```

제네릭이 있기 전의 List는 Object를 담기 때문에 다양한 자료형이 들어갈 수 있었고 이는 잠재적 에러를 발생시키는 문제가 될 수 있습니다.


## 03. 제네릭 사용법

제네릭은 클래스 옆에 꺽쇠 괄호(`<>`) 로 표현합니다.  그리고 이 꺽쇠 괄호를 다이아몬드 라고 부릅니다.
꺽쇠 괄호(`<>`)에는 타입 매개변수(타입 변수)가 들어갑니다.  변수명이 정해져 있는 것은 아니지만 일반적으로 아래의 종류를 사용합니다.
1. E : 요소
2. K : 키
3. N : 숫자
4. T : 타입
5. V : 값
6. S, U, V 등 : 2차, 3차 타입

제네릭 클래스 예제
```java

publoc class Box<T> {
	private T t;

	public void set(T t) {
		this.t = t;
	}

	public T get(){
		return this.t;
	}
}
```

제네릭 인스턴스 생성 예제
```java

Box<String> stringBox = new Box<String>();

/*
자바 7버전 이후 부터는 컴파일러가 유형을 추론하기 때문에 생성자의 타입 변수를 생략할 수 있습니다.
이 때에는 생성자의 타입 변수를 빈 꺽쇠로 표시합니다.
*/
Box<Integer> integerBox = new Box<>();

```

## 04. 원시 타입(Raw Type)

제네릭을 사용하는 클래스에서 타입 변수를 사용하지 않으면 해당 객체는 원시 타입이 됩니다.
제네릭을 사용하는 가장 보편적인 객체인 List와 Map을 예시로 하겠습니다.  

예제)  
```java

List<Integer> list = new ArrayList<>();

// rawList는 List의 원시 타입
List rawList = new ArrayList(); 
/*
아래의 경고 발생
ArrayList is a raw type. References to generic type ArrayList<E> should be parameterized
*/ 

Map<String, String> map = new HashMap<>();

// rawMap 은 Map의 원시 타입
Map rawMap = new HashMap();
/*
아래의 경고 발생
HashMap is a raw type. References to generic type HashMap<K,V> should be parameterized
*/
```

위의 예제에서 rawList와 rawMap은 원시 타입을 가지며 타입 변수는 Object인 것으로 생각하면 됩니다. 실제로 Object를 받는 인스턴스기도 합니다.  
모든 자료를 Object로 받을 때는 매번 형변환을 해줘야하며 안전하지 않기 때문에 원시 타입 객체는 사용하는 것을 지양합니다.

타입 변수를 넣은 객체를 원시 타입 객체에는 할당할 수 있습니다.  
반대의 경우도 가능합니다 하지만 형변환 에러를 유발할 수 있는 잠재적 위험을 가집니다.  

가령 아래의 예제의 경우
```java

List rawList = new ArrayList();
List<Integer> integerList = new ArrayList<>();

//...생략

integerList = rawList;
/*
아래의 경고 발생
Type safety: The expression of type List needs unchecked conversion to conform to List<Integer>
*/

```

여기서 생략된 코드 내에서 rawList에 어떠한 값이 담기는지 우리는 알 수 없습니다. 이는 런타임 에러를 유발하는 위험한 코드입니다.

---
참고문서 : https://docs.oracle.com/javase/tutorial/java/generics/index.html