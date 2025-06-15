---
title: Node에 Redis 적용하기
date: 2025-06-14 00:52
categories:
  - Node.js
tags:
  - Node
  - Redis
_sort:
---
안녕하세요 🐸  

이번엔 제가 노드에 레디스를 연동했던 경험을 기록해놓기 위해 정리합니다.  

## 왜 레디스?
세션은 서버가 다운되거나 재시작되면 정보가 사라지게 됩니다.  
그리고 세션은 서버마다 각자 관리하기 때문에 별도의 조치가 없다면 A 서버에서 로그인 했던 사용자가 다음 요청은 B 서버로 요청되어서 로그인이 되지 않은 사용자로 식별 될 수 있습니다.  
이러한 복합적인 문제를 해결하기 위해 세션 저장소를 레디스라는 별도의 데이터베이스를 사용하는 것 입니다.

## 필요 패키지 설치

> npm i redis connect-redis


## 레디스 계정 생성 및 데이터베이스 생성
<https://redis.io/ko/>  
에서 레디스 계정을 생성할 수 있습니다. 구글, 깃허브 등으로도 가입이 손쉽게 가능합니다.  

![](/assets/img/screenshot/Pasted%20image%2020250614010658.png)  
가입했다면 좌측의 데이터베이스를 선택하고 프리 플랜을 선택합니다.  
저는 이미 프리 플랜인 데이터베이스를 하나 사용 중이어서 비활성화 되어있습니다.  

다음으로 봐야하는 부분은 데이터베이스 이름과 클라우드 벤더입니다.  
![](/assets/img/screenshot/Pasted%20image%2020250614011028.png)  
<br>
어차피 프리 플랜만 이용할 예정이니 저는 AWS 로 했고 리전은 서울로 했습니다.  
만약 유료 사용을 한다면 벤더에 따라 가격이 달라지므로 잘 비교해보시고 선택하세요.  

생성한 데이터베이스에 접근해서 연결을 위해 사용하는 것은 두 가지인데요  
첫 번째로 아래 화면처럼 General 영역의 Public endpoint 입니다.  
![](/assets/img/screenshot/Pasted%20image%2020250614011923.png)  
엔드포인트와 포트가 명시되어 있습니다.  

두 번째로는 Security 영역의 password 입니다.  
![](/assets/img/screenshot/Pasted%20image%2020250614011958.png)  

이 두 값을 사용해서 레디스 데이터베이스에 연결할 수 있습니다.  

## app.js 수정
```javascript
const {RedisStore} = require('connect-redis');
const {createClient} = require('redis');
const express = require('express');
const app = express();

const redisClient = createClient({
        url:`{Public endpoint}`, // 레디스에서 확인한 Public endpoint
        password : 'password' // 레디스에서 확인한 패스워드
    });
redisClient.on('error', (err) => console.error('Redis Client Error', err));
redisClient.connect();

let redisStore = new RedisStore({
    client: redisClient
})

const sessionOption = {
    /* 다른 옵션 생략*/
    store:redisStore
};
app.use(session(sessionOption));
```
