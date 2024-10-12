---
title: Node.js의 cookie-parser
date: 2024-10-12
categories:
  - javascript
  - Node.js
tags:
  - javascript
  - Node
  - cookie-parser
---
## 목차
1. [cookie-parser 설치 방법](#cookie-parser-설치-방법)
2. [cookie-parser 세팅 방법](#cookie-parser-세팅-방법)
3. [cookie-parser 사용 방법](#cookie-parser-사용-방법)
	1. [쿠키 생성](#쿠키-생성)
	2. [쿠키 읽기](#쿠키-읽기)
	3. [쿠키 삭제](#쿠키-삭제)
4. [특이사항](#특이사항)

---

## cookie-parser 설치 방법

> npm i cookie-parser

---
## cookie-parser 세팅 방법

```javascript
const express = require('express');
const app = express();
const cookieParser = require('cookie-parser');

app.use(cookieParser('secret'))

```

`cookieParser()` 에 `'secret'` 이라는 문자열을 인수로 넣었으며 이는 서명키 입니다.  
쿠키를 서명하기 위해 사용하며 필수 값은 아닙니다.

---

## cookie-parser 사용 방법

### 쿠키 생성
`response` 객체의 `cookie()` 함수로 만들 수 있습니다.  
매개변수는 `키, 값, 옵션` 으로 되어 있습니다.  

```javascript
app.get('/setCookie',(req,res,next)=>{
	res.cookie('key','value',{
		httpOnly:true,
		signed:true
	});
})
```

옵션의 종류는 다음과 같습니다.

| 이름       | 기능                  | 기본값                                 |
| -------- | ------------------- | ----------------------------------- |
| maxAge   | 쿠키의 수명 설정 (밀리 초 단위) | 없음. 브라우저 종료 시 삭제                    |
| expires  | 쿠키의 만료 날짜           | 없음. 브라우저 종료 시 삭제                    |
| httpOnly | 웹서버만 접근 가능 여부       | false                               |
| secure   | https 연결을 통한 쿠키 전송  | false                               |
| domain   | 쿠키가 유효한 도메인         | 현재 도메인<br>ex) 로컬에서 할 경우 `localhost` |
| path     | 쿠키가 유효한 URL 경로      | `/`                                 |
| signed   | 쿠키의 서명 여부           | false                               |

### 쿠키 읽기
`request` 객체에서 확인할 수 있습니다.  
서명된 쿠키와 서명되지 않은 쿠키의 접근 방법이 다릅니다.
- 서명된 쿠키 : `req.signedCookies`
- 서명되지 않은 쿠키 : `req.cookies`

#### 서명된 쿠키 읽기
```javascript
app.get('/getCookie',(req,res,next)=>{
	console.log(req.signedCookies)
	next();
})
```

#### 서명되지 않은 쿠키 읽기
```javascript
app.get('/getCookie',(req,res,next)=>{
	console.log(req.cookies)
	next();
})
```

### 쿠키 삭제
`response` 객체의 `clearCookie()` 함수를 통해 삭제합니다.  
**이름과 옵션이** 일치해야 삭제 됩니다.  

```javascript
app.get('/clearCookie/:key',(req,res)=>{  
    res.clearCookie(req.params.key);  
    res.send(`${req.params.key} (이)가 삭제되었습니다`);  
})
```

**단, `maxAge` , `expires`  는 상관이 없습니다**  

---

## 특이사항
기존의 강의 및 기타 정보를 확인해보면 `maxAge` , `expires` 만 예외 사항이라고 합니다. 공식 문서에서도 저 두가지만을 얘기하고 있네요

>Web browsers and other compliant clients will only clear the cookie if the given `options` is identical to those given to [res.cookie()](https://expressjs.com/en/5x/api.html#res.cookie), excluding `expires` and `maxAge`.


하지만 실제 테스트 해보면 `signed` 도 이름만 일치하면 쿠키를 삭제했습니다.
이 부분에 대해서는 명확한 정보를 찾을 수가 없어 직접 테스트를 해봐야 했습니다.  

다음과 같은 코드를 작성했습니다.  

```javascript
app.get('/setCookie',(req,res)=>{
    res.cookie('name','이름',{
        httpOnly:true,
    });
    res.cookie('age','나이',{
        httpOnly:true,
        signed : true
    })
    res.send('쿠키가 설정되었습니다')
})

app.get('/clearCookie/:key',(req,res)=>{
    res.clearCookie(req.params.key);
    res.send(`${req.params.key} (이)가 삭제되었습니다`);
})
```

우선, 저는 3000번 포트를 사용하고 있어서
위와 같이 있을 때 `localhost:3000/setCookie` 로 연결하여 쿠키 생성을 확인했습니다.  

![](assets/img/screenshot/Pasted%20image%2020241012195546.png)  

그리고 `localhost:3000/clearCookie/age` 로 연결하여 `age` 쿠키를 삭제하고자 했습니다. 하지만 `age` 쿠키는 서명된 쿠키라서 `signed:true` 옵션이 필요했습니다. 하지만 실제로 테스트해본 결과 `age` 쿠키가 삭제되었음을 확인할 수 있었습니다.

![](assets/img/screenshot/Pasted%20image%2020241012200155.png)  

혹시 `clearCookie()` 할 때 `signed:true` 임을 명시해야 하는 것은 아닌지 싶어 다음의 코드로 수정하여 다시 테스트 해보았습니다.

```javascript
app.get('/clearCookie/:key',(req,res)=>{  
    res.clearCookie(req.params.key,{signed:false});  
    res.send(`${req.params.key} (이)가 삭제되었습니다`);  
})
```

이처럼 signed 를 명시하고 `localhost:3000/clearCookie/age` 로 다시 요청을 했습니다.  
결과는 동일하게 `age` 쿠키는 삭제되었습니다. 왜....왤까요...?  

문제는 발견했으나 원인과 해결 방안을 찾지 못한 상태로 끝나는 글이 되어버려 쓰는 이도 읽는 이도 매우 불편한 글이 되어버렸습니다. 혹시 원인을 알고 있다면 알려주세요