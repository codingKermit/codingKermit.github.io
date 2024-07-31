---
title: 제네릭 공식문서 파해치기 (9)
date: 2024-07-24
categories:
  - Java
tags:
  - Java
  - Generic
---
안녕하세요 오래만에 또 공부한 내용을 정리해 봅니다.  

보잘 것 없는 영어 실력으로 번역기 알음알음 돌려가며 봤기 때문에 오역이 있을 수 있습니다.  

최대한 혼자서 해보려 했지만 영어가 막혔을 때 큰 도움이 되었던 **인파_** 님의 블로그가 정말 대단히 큰 도움이 되었습니다.  

## Non-Reifiable Types (비실체화 타입)
컴파일러가 타입소거하여 타입 정보가 제거되는 타입을 의미합니다.  
무제한 와일드 카드를 제외한 모든 제니릭 타입으로 볼 수 있습니다.  

## reifiable type (실체화 타입)
실체화 타입은 런타임에 타입 정보를 온전히 사용하는 타입을 의미합니다.  
여기에는
- 기본 타입 
	- ex) int, boolean, double 
- 비제네릭 타입
	- ex) String, Number, Integer
- Raw Type
	- ex) Map, List, HashMap, ArrayList
- 무제한 와일드카드
가 포함됩니다.

## Heap Pollution (힙 오염)
힙 오염은 힙 영역에 모종의 이유로 잘못된 타입의 객체가 저장되거나 처리되는 것을 의미합니다.  
공식 문서에서는 

> 힙 오염은 파라미터화된 타입의 변수에 그 타입과 맞지 않는 객체가 참조될 때 발생합니다.

라고 설명 하고 있으며 아래와 같은 경우 발생할 수 있다 하고 있습니다.

> 원시 유형과 매개변수화된 유형을 혼합하거나 검사되지 않은 캐스트를 수행할 때 힙 오염이 발생합니다.

하지만 이해가 어렵습니다. 공식 문서의 예제를 보겠습니다.  

먼저 아래와 같은 코드가 있다고 가정하겠습니다.  
#### 매개 변수의 가변 인수 메서드 예제)
```java
public class ArrayBuilder {

  public static <T> void addToList (List<T> listArg, T... elements) {
    for (T x : elements) {
      listArg.add(x);
    }
  }

  public static void faultyMethod(List<String>... l) {
    Object[] objectArray = l;     // Valid
    objectArray[0] = Arrays.asList(42);
    String s = l[0].get(0);       // ClassCastException thrown here
  }

}
```  

여기서 `faultyMethod` 메서드는 ClassCastException이 발생하게 됩니다.  
에러 발생의 원인을 순서대로 따라가 본다면 아래의 순서와 같습니다.
1. 가변 인수인 `List<String>... l` 은 컴파일 타임에 `List<String>[] l` 으로 사용됩니다. 
2. 이를 `Object[]` 로 업캐스팅 합니다.
3. 이후 업캐스팅 된 변수 `objectArray` 의 0 번째에 `Arrays.asList(42)`로 인해 `List<Integer>` 가 저장 됩니다.
4. `l[0].get(0)` 의 호출 결과는 앞선 `Arrays.asList(42)`로 인해 `Integer` 타입을 갖게 되지만 `String` 으로 받아 `ClassCastException` 이 발생합니다.

이제 위의 문제가 있는 코드를 사용하는 구문이 있다 가정하겠습니다.  

#### 매개 변수의 가변 인수 사용 예제)
```java
List<String> stringListA = new ArrayList<String>();
List<String> stringListB = new ArrayList<String>();

ArrayBuilder.addToList(stringListA, "Seven", "Eight", "Nine");
ArrayBuilder.addToList(stringListB, "Ten", "Eleven", "Twelve");
List<List<String>> listOfStringLists =
  new ArrayList<List<String>>();
ArrayBuilder.addToList(listOfStringLists,
  stringListA, stringListB);

ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
  ```  

문서에서는 위와 같이 호출할 때 `ArrayBuilder.addToList` 구문에서 아래의 경고를 발생 시킨다고 합니다.

> `warning: [varargs] Possible heap pollution from parameterized vararg type T`

IDE에 따라 다른 것인지는 모르겠지만 저의 경우에는 아래의 경고를 발생 시킵니다.

> `Type safety: A generic array of List<String> is created for a varargs parameter` 

결국 가변 인수가 문제가 된다는 것은 동일한 맥락입니다.  

`faultyMethod()` 가 경고를 발생시키는 부분은 손쉽게 이해할 수 있습니다. 코드를 보면 에러가 발생할 수 밖에 없는 코드니까요.  하지만 `ArrayBuilder.addToList` 부분은 어째서 경고가 발생하는 것인지 이해가 바로 되지 않습니다.  이 부분에 대해 좀 더 알아보겠습니다.

자바에서는 매개 변수의 배열을 허용하지 않습니다.  가변 인수 제네릭은 컴파일 단계에서 `Object[]` 가 되기 때문에 어떤 타입이던지 저장할 수 있기 때문에 힙 오염을 발생시킬 위험이 있기 때문입니다.  
매개 변수를 가변 인수로 사용할 경우 컴파일러에서는 `T... elements` 를 배열로 변환하여 `T[] elements`가 됩니다. 이후 타입 소거 단계에서 `T[] elements`는 `Object[] elements` 가 되어 결과적으로는 `Object[] elements` 가 되어버리는 것 입니다.  


`ArrayBuilder.addToList` 에서 경고가 발생되는 부분은 코드가 틀렸기 때문에 발생하는 경고가 아니기 때문에 이해를 하기 위해선
**에러를 발생시킬 수 있는 위험한 코드이기 때문에 우리는 이렇게 작성하지 않기로 약속했기 때문에**  경고를 발생시킨다고 생각하면 되겠습니다.  
<small>사실 저는 직전까지만해도 경고는 정말 코드에 문제가 있으면 컴파일러가 발생시키는 것이라 생각했습니다.</small>  

### 비실체화 타입 가변 인수를 사용하는 메서드 경고 방지하기

1. `@SafeVarargs` 어노테이션 사용하기
2. `@SuppressWarnings({"unchecked", "varargs"})` 어노테이션 사용하기


공식 문서의 내용을 그대로 번역하기에는 저도 이해가 되지 않는 부분이 많습니다. GPT를 활용했고 일부 의역이 있기 때문에 잘못된 부분이 있을 수 있습니다.

---
참조 : [☕ 자바 제네릭 타입 소거 컴파일 과정 알아보기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A0%9C%EB%84%A4%EB%A6%AD-%ED%83%80%EC%9E%85-%EC%86%8C%EA%B1%B0-%EC%BB%B4%ED%8C%8C%EC%9D%BC-%EA%B3%BC%EC%A0%95-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0#reifiable_%EC%99%80_non-reifiable)
참조 : [☕ 힙 오염 (Heap Pollution) 이란?](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A0%9C%EB%84%A4%EB%A6%AD-%ED%9E%99-%EC%98%A4%EC%97%BC-Heap-Pollution-%EC%9D%B4%EB%9E%80)
참조 : [자바 공식 문서(Non-Reifiable Types)](https://docs.oracle.com/javase/tutorial/java/generics/nonReifiableVarargsType.html)  
