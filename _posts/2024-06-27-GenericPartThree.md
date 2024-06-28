---
title: 제네릭 공식문서 파해치기 (3)
date: 2024-06-27
categories:
  - Java
tags:
  - Java
  - Generic
---
안녕하세요 🐸

오늘은 타입 변수를 제한하는 `extends` 에 대해 정리해보겠습니다.  

### extends 목적

제네릭은 개발자가 타입 변수를 직접 입력하여 사용하기 때문에 코드의 유연성이 늘어납니다.  

하지만 상황에 따라서는 타입을 제한을 해야할 경우가 있습니다.

예를 들어 Number를 구현한 클래스만을 받아야한다든지 하는 상황이 그런 예일 수 있겠습니다.

### 기본 사용 방법

꺽쇠 내에 변수와 상속되는 클래스를 명시하여 사용합니다

사용 예제

```
public class Multipler<T extends Number>{
	private T t;

	public void set(T t){
		this.t = t;
	}

	public T get(){
		return t;
	}

	public <U extends Number> void multiple(U u){
		System.out.print(u + t)
	}

}
```

Number를 상속받은 클래스에 한해 타입 변수를 받도록 작성한 예제입니다.

이처럼 어떠한 클래스를 구현한 클래스에 한해서 타입 변수를 받고 싶을 때에 사용이 가능합니다.

### 컴파일 에러 발생

위의 경우에서 `multiple()` 메서드의 타입 변수는 Number를 상속받은 객체로 한정하기 때문에 아래의 경우 컴파일 에러를 발생합니다.

```

Multipler<Integer> multipler = new Multipler();

multipler.set(new Integer(20));
multipler.multiple("곱하기 5"); // 컴파일 에러 발생

```

### 제한한 타입의 메서드 사용

`extends` 를 통해 타입 변수의 자료형을 제한했기 때문에 우리는 변수의 자료형이 어떤 자료형인지 유추할 수 있습니다. 그리고 이는 컴파일러에서도 동일하여 `extends` 한 클래스의 메서드를 사용할 수도 있습니다.

앞선 예제에서는 Number를 상속받은 클래스로 타입을 제한했기 때문에 Number에서 제공하는 메서드를 사용할 수 있습니다.   
예를 들자면 아래와 같습니다.

```
public class Multipler<T extends Number>{
	...생략
	
	public <U extends Number>void methodFromNumber(U u) {
		System.out.println(u.byteValue());
		System.out.println(u.doubleValue());
		System.out.println(u.intValue());
		System.out.println(u.floatValue());
		System.out.println(u.longValue());
		System.out.println(u.shortValue());
	}
}

```

위의 예제에서는 Number를 상속받은 타입을 자료형으로 받기 때문에 Number의 메서드를 사용할 수 있습니다.  

**주의점**  
여기서 주의할 것은 `extends`를 한 타입의 메서드만 사용이 가능하다는 것입니다.  
예를 들어 Number를 상속받은 클래스에는 Intger가 있습니다. 하지만 Number에서는 구현되지 않고 Integer에서만 구현된 메서드는 사용할 수 없습니다.

### 다중 바운드

앞선 예시에서는 Number를 상속받은 클래스로 제한을 했습니다.  
경우에 따라서는 Number뿐만 아니라 다른 인터페이스까지 구현을 한 클래스로 제한을 해야하는 경우도 발생할 수 있습니다.

다음은 Number와 Comparable을 구현한 클래스로 제한하는 다중 바운드 예제 입니다.

```
public class MultipleBoundTest {
	
	public static <T extends Number & Comparable<T>> int compare(T t1, T t2) {
		return t1.compareTo(t2);
	}
}
```

위의 조건에 해당하는 클래스는 Integer, Double 등이 있지만 테스트를 위해 임시로 객체를 직접 생성해보았습니다.  

```
public class CustomNum extends Number implements Comparable<CustomNum>{
	int i;
	
	public void set(int i) {
		this.i = i;
	}
	
	public int get() {
		return i;
	}
	
	@Override
	public int compareTo(CustomNum o) {
		if(o.get() > i) {
			return 1;
		}else if (o.get() < i){
			return -1;
		} else {
			return 0;
		}
	} // this보다 
	...생략
}
```

그리고 이를 테스트 실행해보는 코드입니다.

```
public void main(String[] args){
		CustomNum a = new CustomNum();
		CustomNum b = new CustomNum();
		
		a.set(10);
		b.set(20);
		
		System.out.println(
				MultipleBoundTest.compare(a, b)
		); // 1 출력
}
```

