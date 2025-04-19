---
title: supertest 로 통합 테스트 하기
date: 2025-04-08
categories:
  - Node.js
tags:
  - Node
  - express
  - supertest
---
안녕하세요 🐸  

supertest 라이브러리를 통해 통합 테스트 하는 방법을 정리해보겠습니다.  

## supertest
통합테스트를 하기 위한 라이브러리로 라우트를 테스트 할 수 있습니다.

## 설치 방법

```
npm i -D supertest
```

## 사용 방법
supertest 를 사용하기 위한 작업을 몇 단계로 요약하자면 다음과 같습니다.  
1. app.js 에서 app을 export
2. app.js 에서 서버 실행 코드를 분리
3. package.json 에서 start 스크립트의 변경
4. 테스트 코드 작성 및 테스트

### app.js 에서 app을 export
기존에 app.js 에서 서버를 실행시키는 아래의 코드를 제거합니다.  

```javascript
// 생략
app.listen(/*내용 생략*/);
```

그리고 생성한 app 을 `module.exports` 합니다.  
예시)  

```javascript
// 생략
module.exports = app;
```

이를 통해 app.js 는 자체적으로 서버를 실행시키는 것이 아니고 서버의 설정을 완성만 하고 이것을 export 하는 역할을 하게 됩니다.  
### app.js 에서 서버 실행 코드를 분리
앞 단계를 통해 app.js 는 더 이상 자체적으로 서버를 실행시키지 않습니다.  
아래의 코드가 제거되었기 때문입니다.  

```javascript
app.listen(/*내용 생략*/);
```

이제 실제로 서버를 시작해줄 역할이 필요합니다.  
그것을 임의의 파일 `server.js` 로 명명지어 사용하겠습니다.  
server.js 에서는 app.js 에서 export한 app을 받아서 서버를 시작합니다.  
코드는 아래와 같습니다.  

```javascript
const app = require('./app');
app.listen(app.get('port'),()=>{
    console.log(app.get('port'),'번 포트에서 대기 중');
})
```


기존에는 파일 구조가 아래와 같았습니다.  
- / (root)
	- app.js
	- package.json
	- 기타 파일 및 디렉토리

이런 파일 구조를 다음과 같이 수정합니다.  
- / (root)
	- app.js
	- server.js
	- package.json
	- 기타 파일 및 디렉토리

여기서 새로이 추가된 `server.js` 라는 파일은 서버를 실행시키는 역할만을 담당하게 됩니다.  

### package.json 에서 start 스크립트의 변경
이제 서버를 실행시키는 파일이 변경되었기 때문에 서버를 시작하는 스크립트의 내용도 변경되어야 합니다.  
기존 package.json 파일의 스크립트에서 start 는 app.js 파일을 사용 하고 있었습니다.  
이를 실제로 서버를 시작하는 `server.js` 파일로 변경합니다.  

기존 : `"start":"nodemon app.js"`  
변경 : `"start":"nodemon server.js"`

### 테스트 코드 작성 및 테스트
테스트 코드 파일의 생성 방법은 jest 와 동일하게 파일 명에 `.test` 혹은 `.spec` 을 사용하는 것 입니다.  
예시)  
- `user.js` 의 테스트 파일 `user.test.js`  

테스트를 하기에 앞서 다음의 4개의 함수를 통해 테스트 전/후 처리를 할 수 있습니다.  
- beforeAll : 블럭 내의 테스트 수행 전 1회 동작
- beforeEach : 블럭 내의 각각의 테스트에 앞서 동작
- afterAll : 블럭 내의 테스트 수행 후 1회 동작
- afterEach : 블럭 내의 각각의 테스트 후에 동작

이후 실제 테스트는 jest와 동일하게 아래의 구성을 가집니다.  

```javascript
describe('테스트 그룹',()=>{
	test('테스트1',(done)=>{});
	test('테스트2',(done)=>{})
})
```

여기서 특이한 부분은 `done` 이라는 콜백 함수의 존재입니다.  
이는 비동기적으로 실행될 때 테스트가 완료되었음을 Jest에 알려주는 함수 입니다.  

실제 테스트를 요청하는 것은 아래의 형식을 가집니다.  

```javascript
const app = require('../app');
const request = require('supertest');
// 생략
request(app).post('/test')
.send({/*body 파라미터*/})
.expect({상태코드},done);
```

일부 테스트에서는 세션을 유지해야하는 경우가 있습니다.  
로그인한 사용자에 대한 로직을 테스트 하기 위해서가 그 예시입니다.  
이 경우 `agent()` 함수를 통해 세션을 유지하는 객체를 생성하여 사용합니다.  

아래는 예시입니다.  

```javascript
const app = require('../app');
const request = require('supertest');
// 생략
describe('세션 유지',()=>{
	const agent = request.agent(app);
	test('로그인 테스트',(done)=>{
		agent.post('/login')
		.send({/*사용자정보*/})
		.expect(302,done);
	})
	test('로그인 사용자 확인',(done)=>{
		agent.post('/login/test')
		.expect(302,done);
	})
});
```


아래는 supertest 에서 주로 사용되는 함수들 입니다.
- beforeAll : 블럭 내의 테스트 수행 전 1회 동작
- beforeEach : 블럭 내의 각각의 테스트에 앞서 동작
- afterAll : 블럭 내의 테스트 수행 후 1회 동작
- afterEach : 블럭 내의 각각의 테스트 후에 동작
- describe : 테스트를 그룹으로 묶음
- test : 실제 테스트 수행
- send : body에 값 전달
- set : 헤더 설정
- query : URL 쿼리 파라미터 전달
- expect : 테스트 기대 결과
- agent(app) : 세션 등을 유지하면서 app을 재사용 가능
- end : 요청을 실제로 서버로 보내고 응답을 받음

## 테스트 코드 작성 예시

아래는 테스트 코드의 예시 입니다.

```javascript
const app = require('../app');
const request = require('supertest');
const {sequelize} = require('sequelize');

// 전체 시작하기 전 1회 수행
beforeAll(async()=>{
	// 테스트를 수행하기 전 DB 연결을 위한 시퀄라이즈 싱크
	// 이전 테스트로 인한 데이터 제거 하기 위한 force:true
	await sequelize.sync({force:true});
})

describe('테스트 그룹1',()=>{
	test('테스트 내용',(done)=>{
		request(app).post('/test')
		.send({
			id:'id',
			password:'password'
		})
		.expect('Location','/')
		.expect(302,done);
	})
})

describe('세션 유지 테스트',()=>{
	const agent = request.agnet(app);
	test('쿠키 생성',(done)=>{
		agent.post('/cookie')
		.send({
			id:'id'
		})
		.expect('Location','/')
		.expect(302,done)
	});

	test('세션을 필요로 하는 테스트',(done)=>{
		agent.post('/cookie/test')
		.expect(200,done);
	});
})

afterAll(async()=>{
	await sequelize.close();
})
```
