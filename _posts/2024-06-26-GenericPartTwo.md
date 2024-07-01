---
title: 제네릭 공식문서 파해치기 (2)
date: 2024-06-26
categories:
  - Java
tags:
  - BackEnd
  - Java
  - 제네릭
  - Generic
---
안녕하세요 🐸

오늘은 제네릭 메서드에 대해 정리해보겠습니다.  

제네릭 메서드는 제네릭의 다른 항목에 비해 이해하기가 어려워서 작성하는데 시간이 걸렸네요

### 01. 제네릭 메서드 (Generic Method)

제네릭 메서드는 타입 변수를 통해 매개 변수, 반환 타입을 결정하는 메서드 입니다.

제네릭 메서드와 제네릭 클래스는 서로 독립적입니다. 
제네릭 메서드를 구현한 클래스가 제네릭 클래스인지는 상관이 없다는 얘기입니다.

제네릭 메서드의 예시
```java

public class Util{

	public static <T> printArray(T[] t){
		for(T item : t){
			System.out.print(item);
		}
	}
}

... 생략
public static voie main(String[] args){
	String stringArray = {"pizza","chicken"};
	Integer intArray = {1,2,3,4,5};
	
	Util.<String>printArray(stringArray);
	Util.printArray(intArray);
}

```

#### 타입 변수의 생략
제네릭 메서드는 타입 변수를 생략할 수 있습니다. 매개 변수의 자료형으로 타입을 추론하기 때문입니다.

위에서 예제로 만든 `printArray()` 를 사용할 때 String[]은 앞에 타입 변수를 명시했지만 Integer[]는 타입 변수를 적지 않아도 정상 동작 함을 확인할 수 있습니다.

#### 제네릭 메서드의 장점
이렇게 예제를 봐도 저는 실질적으로 도대체 왜 제네릭 메서드를 쓰는건데? 라는 의구심을 걷어내기가 쉽지 않았습니다.  
그래서 제네릭 메서드를 사용하기 전/후로 예제를 들어보겠습니다.


##### 제네릭 메서드 사용 전

```java

public class Util{

	// 문자열 배열을 출력하는 메서드
	public static void printStringArray(String[] array){
		for(String item : array){
			System.out.println(item);
		}
	}

	// 정수 배열을 출력하는 메서드
	public static void printIntegerArray(Integer[] array){
		for(Integer item : array){
			System.out.println(item);
		}
	}

	// 실수 배열을 출력하는 메서드
	public static void printDoubleArray(Double[] array){
		for(Double item : array){
			System.out.println(item);
		}
	}

}
```

제네릭 메서드를 사용하기 전에는 위의 예제처럼 비슷한 기능을 하면서 중복되는 코드가 많이 발생하게 됩니다.

##### 제네릭 메서드 사용 후
```java

public class Util{

	// 모든 타입의 배열을 출력
	public static <T> printArray(T[] t){
		for(T item : t){
			System.out.print(item);
		}
	}
}

```

제네릭 메서드를 사용하면 코드의 유연성이 증가하고 중복되는 코드를 줄일 수 있습니다.  

이렇게 예제를 보니 확실히 체감이 되네요!

---
참고문서 : https://docs.oracle.com/javase/tutorial/java/generics/index.html