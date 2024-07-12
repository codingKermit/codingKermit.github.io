---
title: 제네릭 공식문서 파해치기 (6)
date: 2024-07-11
categories:
  - Java
tags:
  - Java
  - Generic
---
안녕하세요 🐸!  

공부에 헤이해진 요즘 다시 정신 다잡고 공부 시작합니다!  

## 와일드카드와 하위 유형
이전에 제네릭의 상속에 대해 봤었습니다.  
제네릭은 타입 변수 간의 상속이 제네릭 클래스에서 관계와 연결되는 것은 아님을 봤습니다.  
예로 `List<Number>` 와 `List<Integer`는 서로 상속 관계에 있지 않습니다. 그리고 이 둘의 공통 부모는 `List<?>` 가 됩니다.   `List<?>`는 `List<? extends Object>` 로 볼 수 있기 때문에 `List<Number>`와 `List<Integer>`의 공통 조상이 되는 것 입니다.  
아래의 표로 이 관계를 표현할 수 있습니다.
![](https://wsrv.nl?url=https://docs.oracle.com/javase/tutorial/figures/java/generics-listParent.gif)  

그리고 이전에 하한, 상한 와일드카드에서 공부한 것을 접목하면 아래와 같은 관계가 성립됩니다.
![](https://wsrv.nl?url=https://docs.oracle.com/javase/tutorial/figures/java/generics-wildcardSubtyping.gif)  

## 와일드카드 캡쳐 & 헬퍼 메서드
### 와일드카드 캡쳐
컴파일러가 와일드카드의 타입을 추론하는 것을 `Wildcard Capture` 라고 부릅니다.  
와일드카드 캡쳐 에러 핸들링은 공식 문서에서도 크게 신경쓰지 않아도 괜찮은 것으로 얘기하지만, 그래도 공식문서 정리하기니까 한번 보고 넘어가겠습니다.

> 컴파일 에러에서 `capture of`라는 키워드가 나오지 않는 이상 크게 신경 쓸 필요는 없다.
 
공식 문서에서조차 이렇게 얘기하는 것을 보니 실제로 경험할 일이 그리 많지는 않은 것으로 생각 됩니다. 
문서에 있는 와일드카드 에러를 발생시키는 예제입니다.
```java
import java.util.List;

public class WildcardError {

    void foo(List<?> i) {
        i.set(0, i.get(0));
    }
}
```
위의 코드는 아래와 같은 컴파일 에러를 발생 시킵니다.  

`The method set(int, capture#1-of ?) in the type List<capture#1-of ?> is not applicable for the arguments (int, capture#2-of ?)`  

**왜 컴파일 에러가 발생할까?**
위의 코드에서 `List<?>`는 어떤 자료형이던 들어올 수 있습니다. 이는 컴파일러는 여기에 사용될 구체적 타입을 알 수 없다는 뜻입니다.  
이런 문제를 해결하기 위해 문서에서는 헬퍼 메서드를 소개하고 있습니다.

### 헬퍼 메서드
위에서 설명한 헬퍼 메서드를 통해 캡쳐 에러를 해결하는 코드입니다.  
```java
public class WildcardFixed {

    void foo(List<?> i) {
        fooHelper(i);
    }


    // Helper method created so that the wildcard can be captured
    // through type inference.
    private <T> void fooHelper(List<T> l) {
        l.set(0, l.get(0));
    }

}
```

위에서 사용한 `fooHelper()`가 헬퍼 메서드의 역할을 수행합니다.  헬퍼 메서드에서 컴파일러는 타입을 추론하여 자료형 T임을 확인합니다.  
여기서 메서드 명명규칙에 대해서도 구체적으로 명시를 해두었는데요 `{메서드명}Helper`  과 같은 규칙을 따르기를 권장하고 있습니다. 

---
**추가**
문서만 보니 제가 잘못 이해하고 있는 부분들이 많아 다시 되짚어가며 이해하기가 어려웠던 부분이 많았습니다.  
다시 되짚어가며 이해했던 부분을 요약 정리해보겠습니다.  

1. 와일드카드(`?`)는 "알 수 없는" 타입이며, 어떤 타입이던 올 수 있다.
   어떤 타입이던지 사용할 수 있기 때문에 컴파일러는 객체의 타입을 특정할 수 없다.
2. 와일드카드에 어떤 타입이든 올 수 있다는 얘기와 `List<Object>` 는 엄연히 다르다.
   `List<Object>`는 타입이 명확하지만 `List<?>`는 타입을 특정할 수 없다.
3. `List<T>`는 컴파일러가 타입을 `T`로 추론하고, `List<?>`는 `List<String>`이 될 수도 있고 `List<Integer>`가 될 수도 있고 `List<Boolean>`이 될 수도 있다. 말 그대로 어떤 타입이던 될 수 있기 때문에 컴파일러는 이를 특정 할 수 없다.