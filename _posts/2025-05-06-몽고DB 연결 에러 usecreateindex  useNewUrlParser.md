---
title: "몽고DB 연결 에러 MongoParseError: option usecreateindex is not supported"
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
aliases:
  - 2025-05-06-몽고DB 연결 에러 usecreateindex
  - useNewUrlParser
---
안녕하세요 🐸  

산넘어 산입니다.  
이번에는 

`MongoParseError: option usecreateindex is not supported` 라는 에러가 뜨네요  
usecreateindex 라는 옵션이 지원이 안된다니 그러면 뭐 없어진건지 다른걸로 대체가 된건지에 대한 자세한 설명이 없길래 공식문서를 들어가서 검색해봤습니다.  

>`useNewUrlParser`, `useUnifiedTopology`, `useFindAndModify`, and `useCreateIndex` are no longer supported options. Mongoose 6 always behaves as if `useNewUrlParser`, `useUnifiedTopology`, and `useCreateIndex` are `true`, and `useFindAndModify` is `false`. Please remove these options from your code.

번역해보면 `useNewUrlParser` , `useUnifiedTopology` , `useFindAndModify` , `useCreateIndex` 이렇게 네 가지 옵션은 더 이상 지원을 안한다고 합니다.  
이 중에 `useNewUrlParser` , `useUnifiedTopology` , `useCreateIndex` 이 세 옵션은 항상 `true` 로 기본 탑재라고 하네요.  
그렇다면 저는 `useCreateIndex` 옵션과 `useNewUrlParser` 를 사용하고 있었으니 이 두 옵션을 지워줘야 겠습니다.  

##### 수정 전
```javascript
mongoose.connect(`mongodb://${process.env.MONGO_ID}:${process.env.MONGO_PASSWORD}@localhost:27016/admin`,{
	dbName:'gifchat',
	useNewUrlParser:true,
	useCreateIndex:true,
}).then((error)=>{
	if(error){
		console.error('몽고 DB 연결 에러',error)
	} else {
		console.log('몽고 DB 연결 성공');
	}
})
```

##### 수정 후
```javascript
mongoose.connect(`mongodb://${process.env.MONGO_ID}:${process.env.MONGO_PASSWORD}@localhost:27016/admin`,{
	dbName:'gifchat',
}).then((error)=>{
	if(error){
		console.error('몽고 DB 연결 에러',error)
	} else {
		console.log('몽고 DB 연결 성공');
	}
})
```


---
참고 : [https://mongoosejs.com/docs/migrating_to_6.html#no-more-deprecation-warning-options](https://mongoosejs.com/docs/migrating_to_6.html#no-more-deprecation-warning-options)  