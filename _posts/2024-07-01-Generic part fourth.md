---
title: 제네릭 공식문서 파해치기 (4)
date: 2024-07-01
categories:
  - Java
tags:
  - Java
  - Generic
---
안녕하세요 🐸!

제네릭의 상속과 하위 유형에 대해 알아보겠습니다.

## 상속 관계
앞서 일반적으로 상속은 아래와 같은 관계를 보입니다.

```java
Object obj = new Object();
Integer integer = new Integer(10);

obj = integer; // OK
```

Integer는 Object의 자식 이기 때문에 Object 객체에 Integer를 할당할 수 있습니다. 이를 고려했을 때 아래의 관계도 제네릭에서 유효합니다.

```java
Box<Number> box = new Box<>();
box.add(new Integer(10)); // ok
box.add(new Double(20.0)); // ok

List<Number> list = new ArrayList<>();
list.add(new Integer(10)); // ok
list.add(new Double(0.1)); // ok
```

이러한 부분 때문에 아래의 관계도 성립이 될 것이라는 착각을 할 수 있습니다.

```java
List<Number> numList = new ArrayList<>();
List<Integer> intList = new ArrayList<>();

numList = intList; // error
```
이 관계를 문서에서는 아래와 같은 표로 정리해주고 있습니다.

![제니릭 상속 관계](https://docs.oracle.com/javase/tutorial/figures/java/generics-subtypeRelationship.gif)

이처럼 상속 관계를 이루는 타입 변수를 갖는 제네릭 클래스 A와 B는 서로 상속 관계에 있지 않습니다.

## 제네릭 클래스의 상속과 하위 계층화

하지만 제네릭은 클래스 자체는 상속이 가능하고 하위 유형을 만들 수도 있습니다.  
가장 대표적인 예는 `Collection` , `List`, `ArrayList` 의 관계가 있습니다.
![](https://docs.oracle.com/javase/tutorial/figures/java/generics-sampleHierarchy.gif)  

코드로 직접 만들어 예제를 만들어 보겠습니다.

아래는 제가 생성한 제네릭 클래스의 관계 입니다.
> Continent(인터페이스) -> Country(인터페이스) -> City(클래스)

```java
	City<String> city = new City<>();
	Country<String> country = new City<>();
	Continent<String> continent = new City<>();

//	city = country; // error
//	country = continent; // error
		
	country = city; // ok
	continent = country; // ok
```

위의 예제 처럼 제네릭 클래스 자체는 서로 상속이 가능하며 계층 구조로 만들 수 있습니다.

## 타입 추론

타입 추론도 이해가 잘 안되어서 글 정리가 힘드네요 

### 타입 추론 이란
자바 컴파일러가 메서드의 호출과 선언부를 보고 타입을 결정하는 기능 입니다.

예제)
```java
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());
```

제네릭 메서드의 타입 변수 생략은 매개 변수의 자료형을 보고 생략이 가능하다고 했습니다.  
하지만 여기서 두 매개 변수는 String과 ArrayList로 서로 다릅니다.  

여기서 반환 값이 `Serializable` 인 것에서 유추를 해볼 수 있는데요, 두 클래스가 공통으로 구현하고 있는 인터페이스는 `Serializable` 입니다.

![String](assets/img/screenshot/Pasted%20image%2020240703143442.png)  
<p><small align="center">String</small></p>

![ArrayList](assets/img/screenshot/Pasted%20image%2020240703143527.png)   
<p><small>ArrayList</small></p>

타입 추론은 가능한 가장 구체적인 클래스로 하게 됩니다.  
때문에 어떠한 두 자료형을 넣었을 때 공통되는 인터페이스 or 상속이 없다면 Object로 추론되곤 합니다.


❓여기서 제가 한가지 궁금했던 점이 있습니다.  **Comparable을 구현한 두 클래스의 경우에는 어떻게 되는가?** 입니다.

조금만 생각보면 유추할 수 있듯이 결국 Object로 추론됩니다.  
예로 String과 Integer는 둘 다 Comparable을 구현하지만 Comparable또한 제네릭 클래스 입니다.  
결국 실제로 String은 `Comparable<String>` 을 구현하며 Integer는 `Comparable<Integer>`를 구현하기 때문에 공통 구현체가 없어 Object로 추론됩니다.