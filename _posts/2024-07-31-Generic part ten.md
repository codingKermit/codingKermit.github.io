---
title: 제네릭 공식문서 파해치기 (10)
date: 2024-07-31
categories:
  - Java
tags:
  - Java
  - Generic
---
## 제네릭 제한 사항

### 01. 기본 타입으로 사용 불가
제네릭은 기본 타입을 사용할 수 있습니다.
아래와 같은 코드가 있다할 때, 컴파일 에러를 발생 시킵니다.
```java
  Pair<int, char> p = new Pair<>(8, 'a');  // compile-time error
```  

위의 코드를 대체 하기 위해서는 래퍼 클래스인 `Integer`, `Character`를 사용해야합니다.  
올바르게 수정된 예시 입니다.  
```java
	Pair<Integer, Character> p = new Pair<>(8, 'a');
```  

해당 단락에서는 오토박싱에 대해서도 언급하고 있습니다.  하지만 이 부분은 추후에 따로 문서를 보면서 정리하겠습니다.

### 매개 변수 인스턴스 생성 불가
매개 변수의 인스턴스를 생성할 수 없습니다. 아래의 코드는 컴파일 에러를 발생시킵니다.
```java
public static <E> void append(List<E> list) {
	E elem = new E();  // compile-time error
	list.add(elem);
}
```  

리플렉션을 통해 매개 변수의 객체를 생성할 수는 있습니다.  
```java
public static <E> void append(List<E> list, Class<E> cls) throws Exception{
	E elem = cls.newInstance();   // OK
	list.add(elem);
}
```  
위의 경우 `append()`를 다음과 같이 호출하여 사용할 수 있습니다.  
```java
List<String> ls = new ArrayList<>();
append(ls, String.class);
```

**리플렉션에 대해서는 따로 작성하겠습니다**
### 매개 변수 타입의 정적 필드 생성 불가
매개 변수 타입의 정적 필드는 사용할 수 없습니다.  다음의 코드는 컴파일 에러를 발생시킵니다.  
```java
  public class MobileDevice<T> {
	  private static T os;
  }
```  
이 처럼 매개 변수 타입의 정적 필드가 허용 된다면 다음과 같을 때 문제가 발생할 수 있습니다.  
```java
	MobileDevice<Smartphone> phone = new MobileDevice<>();
	MobileDevice<Pager> pager = new MobileDevice<>();
	MobileDevice<TabletPC> pc = new MobileDevice<>();
```  
이 때 `os`의 타입은 무엇인지 알 수 없습니다. 그렇기 때문에 매개 변수 타입의 정적 필드는 사용할 수 없습니다.

### 캐스트 및 `instanceof` 사용 불가
컴파일러의 제네릭 타입 소거로 인해 런타임 단계에서는 어떤 매개 변수가 사용되었는지 알 수 없습니다.
```java
	public static <E> void rtti(List<E> list) {
		if (list instanceof ArrayList<Integer>) {  // compile-time error
			// ...
		}
	}	  
```

무제한 외일드 카드 `<?>` 는 실체화(reifiable) 타입이기 때문에  사용할 수 있습니다  
```java
public static void rtti(List<?> list) {
	if (list instanceof ArrayList<?>) {  // OK;
	}
}
```

그리고 캐스팅도 동일합니다.  
```java
List<Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error
```

하지만 다음과 같은 경우에는 가능합니다.  
```java
List<String> l1 = ...;
ArrayList<String> l2 = (ArrayList<String>)l1;  // OK
```

이 부분은 제네릭 T가 같은 경우 `List`와 `ArrayList`는 상속 관계에 있기 때문입니다.  
앞서 제네릭의 상속 관계에 대해 공부할 때 나왔던 부분입니다.
	
실체화 타입과 비실체화 타입으로 구분지어 생각하니 이해가 쉬웠습니다.

|            | 실체화타입(reifiable) | 비실체화 타입(Non-reifiable) |
| ---------- | :--------------: | :--------------------: |
| instanceof |        가능        |           불가           |
| 캐스팅        |        가능        |           불가           |
 
### 매개 변수 타입의 배열 생성 불가
다음의 코드는 컴파일 에러를 발생시킵니다.
```java
  List<Integer>[] arrayOfLists = new List<Integer>[2];
```  
이 또한 타입 소거로 인해 발생하는 문제 입니다.  

문제를 이해하기 위해 컴파일 에러를 발생 시키는 아래의 코드를 보겠습니다.  
```java
	Object[] strings = new String[2];
	strings[0] = "hi";   // OK
	strings[1] = 100;    // An ArrayStoreException is thrown.
```  
이 구문에서 `String[]` 에 `int`가 저장되려고 하니 컴파일 에러를 발생시킵니다.  이 것이 정상적으로 개발자의 오류를 막아주고 있는 상태입니다.  

하지만 만약 제네릭의 배열을 허용한다면 다음의 코드와 같은 에러가 발생할 수 있습니다.  
```java
	Object[] stringLists = new List<String>[2];
	stringLists[0] = new ArrayList<String>();
	stringLists[1] = new ArrayList<Integer>();
```
만약 제네릭의 배열이 허용된다면 위 코드는 컴파일 에러를 발생시키지 않아 예상치 못한 에러를 발생 시킬 수 있습니다.  
위 코드는 컴파일시 타입 소거로 인해 `List[]` 타입이 될 것이고 결국  `stringLists[1] = new ArrayList<Integer>();`의 구문은 이를 감지하지 못하고 에러를 발생시키는 코드가 될 것 입니다.

### 매개 변수 타입의 객체 생성, catch 및 throw 불가
제네릭 클래승는 Throwable을 직/간접적으로 확장할 수 없습니다.
  아래의 두 예제는 모두 컴파일 에러를 발생시킵니다.
  `Throwable`을 간접적으로 확장받는 예졔)
  ```java
  class MathException<T> extends Exception { /* ... */ }
```
`Exception`은 `Throwable`을 확장받았기 때문에 간접적으로 `Throwable`을 확장하게 됩니다.

`throwable`을 직접적으로 확장받는 예제)
```java
  class QueueFullException<T> extends Throwable { /* ... */}
```

위와 같이 `Throwable`을 확장한 제네릭 클래스를 허용하게 되면 런타임은 실제로 어떤 타입인지 구체적으로 알 수 없기 때문에 이를 허용하지 않습니다.

그리고 다음과 같이 매개 변수 타입을 `catch()`할 수 는 없습니다.
```java
	public static <T extends Exception, J> void execute(List<J> jobs) {
		try {
			for (J job : jobs)
				// ...
		} catch (T e) {   // compile-time error
			// ...
		}
	}
``` 
여기서도 위와 동일하게 제네릭의 타입소거로 인해 `T`의 타입을 런타임에서 어떤 예외 타입인지 알 수 없기 때문에 허용하지 않습니다.

하지만 `throws` 절에서는 사용이 가능합니다.  
다음의 예제를 확인해보겠습니다.
```java
	class Parser<T extends Exception> {
		public void parse() throws T {     // OK
			// ...
		}
	}
```  

여기서는 제네릭 매개 변수 `T`의 타입이 `Exception`의 하위 타입으로 명확하게 정해져있습니다.  
그래서 해당 코드는 유효하며 이런 방법으로 `throws`할 경우 사용하는 측에서는 다음과 같이 `catch()` 문을 작성할 수 있습니다.

```java
try{
	parse();
}catch(IOEexception e){
	System.out.println("Caught IOException: " + e.getMessage());
}catch(Exception e){
	System.out.println("Caught Exception: " + e.getMessage());
}
```  
위의 코드는 `Exception`의 하위임을 명확히 하고 있기 때문에 특정 예외에 대해 처리하고 다른 예외도 처리하는 것을 볼 수 있습니다.  
매개 변수 타입이 `Exception`의 하위 타입임이 분명하기 때문에 허용됨을 알 수 있습니다.

### 매개 변수 형식이 동일한 Raw Type으로 지워지는 메서드를 오버로딩 불가
타입 소거로 인해 동일한 Raw 타입으로 치환되는 메서드는 오버로딩 할 수 없습니다.  
다음은 타입 소거 후 같은 타입의 메서드가 되어버리는 경우입니다.  
```java
public class Example {
    public void print(Set<String> strSet) { }
    public void print(Set<Integer> intSet) { }
}
```  
이 경우 개발자는 타입을 명시했지만 컴파일러의 타입 소거로 인해 두 메서드는 `Set` 을 인수로 사용하는 메서드가 되어 동일한 메서드가 되어버립니다.


---

#### 제네릭 사용 제한 사항에 대한 생각

대부분의 제한 사항은 타입 소거로 인해 런타임 시 발생할 수 있는 에러를 방지하기 위한 규칙임을 알 수 있었습니다.

