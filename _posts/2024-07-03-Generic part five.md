---
title: 제네릭 공식문서 파해치기 (5)
date: 2024-07-04
categories:
  - Java
tags:
  - Java
  - Generic
---
안녕하세요 ! 🐸

와일드카드 부분은 공식 문서를 읽어도 이해하기가 어려워서 직접 예제 코드를 써보면서 이해하느라 오래 걸리네요.  
논리적 이해가 필요한 파트 입니다.

## 와일드카드

제네릭 코드에서 이따금씩 보이는 `<?>` 가 와일드 카드라고 합니다.  다양한 상황에서 사용할 수 있으며 어떤 타입이든 될 수 있다는 뜻을 가집니다.

## 상한 경계 와일드 카드
상한 경계 와일드카드를 통해 타입의 범위를 제한할 수 있습니다. 키워드는 `extends` 를 사용합니다.  타입 변수를 특정 클래스의 하위 타입으로 제한할 수 있습니다.
공식 문서에서는 아래와 같은 예제를 사용하고 있습니다.
```java
public static void process(List<? extends Foo> list) { /* ... */ }
```
여기서는 `Foo`와 `Foo`를 상속받은 클래스를 타입 변수로 제한하고 있습니다.  
이렇게 보면 앞서 학습했던 `<T extends Foo>` 와 무엇이 다른지 이해가 되지 않습니다.  이해가 되지 않으니 사용하는 이유에 대해서도 납득이 되지 않죠.  

여기서 갓 GPT 에게 적당한 예제를 물어봤습니다.

**와일드 카드 사용 전)**
```java
import java.util.List;
import java.util.ArrayList;

class Box<T> {
    private List<T> items = new ArrayList<>();

    public void addItems(List<T> items) {
        this.items.addAll(items);
    }

    public void printItems() {
        for (T item : items) {
            System.out.println(item);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<>();
        List<Integer> integers = List.of(1, 2, 3, 4, 5);
        
        integerBox.addItems(integers);
        integerBox.printItems();
    }
}

```
위의 경우에서 `addItems()` 메서드는 `List<T>`를 매개 변수로 사용하고 있습니다. 그렇기 때문에 `Box<Integer>` 인스턴스는 `addItems()` 에 `List<Integer>` 만 전달할 수 있습니다.  


💡제가 생각을 너무 짧게 해서 범했던 논리 오류가 두 가지 있었는데요.
첫 번째는
*그러면 `<T extends Number>` 쓰면 되는거 아닌가?*  라는 생각을 했습니다만 여기서 Box의 `addItems()`는 `Number` 의 상속 클래스만을 위한 메서드가 아니기 때문에 이는 논리적 오류였습니다.
두 번째는
*`Box<Integer>` 가 아니라 `Box<Number>` 를 사용하면 되지 않나?* 라는 생각도 했습니다.  
이 생각은 앞서 공부했었던 부분을 망각해서 발생한 오류입니다. `Number` 와 `Integer` 는 서로 상속관계가 맞지만 `List<Number>`와 `List<Integer>` 는 서로 상속 관계가 아니기 때문에 타입이 맞지 않는 매개 변수를 사용한 꼴이 됩니다.  
이 부분이 이해가 되지 않거나 했갈린다면 아래의 글을 다시 읽어보는게 좋겠습니다.
[=>제네릭의 상속 관계](https://codingkermit.github.io/posts/Generic-part-fourth/)  


**와일드 카드 사용 후)**
```java
import java.util.List;
import java.util.ArrayList;

class Box<T> {
    private List<T> items = new ArrayList<>();

    public void addItems(List<? extends T> items) {
        this.items.addAll(items);
    }

    public void printItems() {
        for (T item : items) {
            System.out.println(item);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Box<Number> numberBox = new Box<>();
        List<Integer> integers = List.of(1, 2, 3, 4, 5);
        List<Double> doubles = List.of(1.1, 2.2, 3.3);
        
        numberBox.addItems(integers);
        numberBox.addItems(doubles);
        numberBox.printItems();
    }
}

```

여기서 `addItems()`는 T를 포함, T를 상속한 클래스의 List를 자료형으로 가집니다.  
예제 코드의 경우에는
`Box<Number> numberBox = new Box<>()`에서 `T` 는 `Number`가 되며 `addItems()`는 `Number` 를 상속하는 클래스의 `List` 를 매개변수로 사용할 수 있습니다.

## 하한 경계 와일드 카드
하한 경계 와일드 카드 또한 상한 경계 와일드 카드와 마찬가지로 타입 변수를 제한하는데 사용합니다.  
상한 경계 와일드 카드는 특정 클래스와 그 하위 클래스로 제한했습니다. 하한 경계 와일드 카드는 특정 클래스와 그의 상위 클래스로 타입을 제한합니다.
하한 경계 와일드 카드는 `?` 뒤에 `super` 키워드와 하한 제한 클래스를 명시하여 사용합니다. 

예시)
```java
<? super Number>
```

>**참고**
>`super`키워드와 `extends` 키워드는 함께 사용할 수 없습니다.

**사용 상황 예시**
`Integer` 객체를 리스트에 추가하는 메서드를 추가하고 싶은 경우, 개발자는 코드의 유연성을 위해 `List<Number>`, `List<Object>` 에서도 동작하는 메서드를 만들고 싶을 수 있습니다. 이런 경우 아래와 같이 사용할 수 있습니다.
```java
public static void addNumbers(List<? super Integer> list) { 
    for (int i = 1; i <= 10; i++) { 
        list.add(i); 
    } 
}
```

## 무제한 와일드 카드
무제한 와일드카드는 `?` 키워드를 사용합니다 (ex : `List<?>`).  
무제한 와일드카드에는 어떤 타입이던지 사용할 수 있습니다.  하지만 `List<?>`와 `List<Object>`는 서로 다릅니다.  `List<?>`는 `List<? extends Object>` 와 같음을 유추해 볼 수 있습니다.  
이 부분을 아래의 예제를 통해 알 수 있습니다.  

`List<Object>` 예제)
```java
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}
```
위에서 `printList()`는 `List<Object>`를 출력하려 합니다.  
하지만 앞서 공부했듯이 `List<String>`은 `List<Object>`의 하위 클래스가 아닙니다.  이로인해 에러가 발생합니다.  
그래서 무제한와일드카드(`?`) 를 사용하여 아래와 같이 수정할 수 있습니다.

`List<?>` 예제)
```java
public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}
```
위의 예제에서 어떠한 타입 변수를 가지는 `List`는 전부 `List<Object>`의 하위 유형이 되기 때문에 모든 타입에 대해 출력을 할 수 있습니다.  

**주의 사항**
`List<Object>`와 `List<?>`가 어떻게 다른지 예제를 통해 이해했습니다. 하지만 주의할 점이 있습니다.  
`List<Object>`에는 `Object` 혹은 `Object`의 하위 유형을 삽입할 수 있습니다.  하지만 `List<?>`에는 null만 삽입할 수 있습니다.  
이 부분은 문서 파해치기 후편에서 이어집니다.

---
참고  : [https://docs.oracle.com/javase/tutorial/java/generics/wildcards.html](https://docs.oracle.com/javase/tutorial/java/generics/wildcards.html)  