---
title: "몽고DB 연결 에러 MongooseError: Mongoose.prototype.connect() no longer accepts a callback"
date: 2025-05-06 19:31
categories:
  - DB
  - MongoDB
tags:
  - MongoDB
  - DB
  - NoSQL
  - Error
_sort:
---
안녕하세요 🐸  

공부중 `MongooseError: Mongoose.prototype.connect() no longer accepts a callback` 이런 에러를 겪었습니다.  
보아하니 콜백형식은 더 이상 지원하지 않는다는 것 같네요🤔  

그래서 소스를 보니 아래처럼 주석에서 링크도 연결되어 있었습니다  

```javascript
  /** Opens Mongoose's default connection to MongoDB, see [connections docs](https://mongoosejs.com/docs/connections.html) */
  function connect(uri: string, options?: ConnectOptions): Promise<Mongoose>;
```

그렇다면 링크를 확인해봐야겠죠?  

`connect()` 는 `Promise` 를 반환하기 때문에 `then()` 을 통해서도 처리가 가능하다고 하네요.  

```javascript
mongoose.connect(uri, options, function(error) {
  // Check error in initial connection. There is no 2nd param to the callback.
});

// Or using promises
mongoose.connect(uri, options).then(
  () => { /** ready to use. The `mongoose.connect()` promise resolves to mongoose instance. */ },
  err => { /** handle initial connection error */ }
);
```

그렇다면 위 문서의 내용을 참고하여 코드를 수정해보겠습니다.  

##### 기존
```javascript
mongoose.connect(`mongodb://${process.env.MONGO_ID}:${process.env.MONGO_PASSWORD}@localhost:27016/admin`,
{
/*옵션*/
},(error)=>{
	/*콜백*/
})
```

##### 변경
```javascript
mongoose.connect(`mongodb://${process.env.MONGO_ID}:${process.env.MONGO_PASSWORD}@localhost:27016/admin`,{
/*옵션*/
}).then((error)=>{
	/*콜백*/
})
```

아마 위 에러를 겪으신 분들은 높은 확률로 옵션에 대한 에러가 발생할겁니다.  
저와 같은 수강생분들이시겠죠😊  
다음 글에 해당 에러도 해결 방법을 찾아놨습니다.

---
참고 : [https://mongoosejs.com/docs/connections.html](https://mongoosejs.com/docs/connections.html)  