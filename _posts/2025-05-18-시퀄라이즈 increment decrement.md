---
title: 시퀄라이즈 increment와 decrement
date: 2025-05-18 16:26
categories:
  - Node.js
tags:
  - Node
  - sequelize
  - express
_sort:
---
안녕하세요 🐸  

시퀄라이즈와 같은 ORM은 직접 쿼리를 작성하지 않아도 되는 장점이 있는 반면, 복잡한 쿼리 작업은 어렵다는 단점이 있는데요.  

그래서 시퀄라이즈에서는 `literal()` 을 통해 직접적인 쿼리 작성을 돕기도 합니다.  

UPDATE 구문에서 컬럼 값의 증가/감소를 표현하기 위해 `literal()` 을 사용하기도 하는데요, 이 떄 대신해서 사용할 수 있는 것이 `increment()` 와 `decrement()` 입니다.  

## 사용 방법

1. 모델에서 직접 사용하기
2. 데이터를 찾아 사용하기


##### 모델에서 직접 사용하기
아래와 같이 where 을 통해 모델에서 직접 사용하는 방법이 있습니다.   

```javascript
await User.increment({ age: 5 }, { where: { id: 1 } }); // Will increase age to 15
await User.increment({ age: -5 }, { where: { id: 1 } }); // Will decrease age to 5
```

##### 데이터를 찾아 사용하기
위 방법이 불안하다면 확실하게 증가/가감할 데이터를 찾아서 사용하는 방법도 있습니다.  

```javascript
const jane = await User.create({ name: 'Jane', age: 100, cash: 5000 });
await jane.increment({
  age: 2,
  cash: 100,
});

// If the values are incremented by the same amount, you can use this other syntax as well:
await jane.increment(['age', 'cash'], { by: 2 });
```

---
참조 : [https://sequelize.org/docs/v6/core-concepts/model-querying-basics/#increment-decrement](https://sequelize.org/docs/v6/core-concepts/model-querying-basics/#increment-decrement) 