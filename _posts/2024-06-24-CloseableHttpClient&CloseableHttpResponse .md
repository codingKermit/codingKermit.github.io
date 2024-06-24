---
title: CloseableHttpClient, CloseableHttpResponse 사용 시 주의할 점
date: 2024-06-24
categories:
  - Back End
  - Java
tags:
  - BackEnd
  - Java
---
안녕하세요 🐸

CloseableHttpClient 클래스와 CloseableHttpResponse  클래스는 HTTP 통신을 이용할 때 쓰는 클래스 입니다.

객체는 통신이 끝난 이후 close()를 수행해줘야합니다.

수행하지 않는다면 메모리 누수가 발생합니다.

이를 코드로 하자면 예제처럼 할 수 있습니다
```
CloseableHttpResponse httpclient = null;
CloseableHttpClient response = null;

try{
	...생략
} catch (Exception e){
	
} finally {
	try {
		if(httpclient != null){
			httpclient.close();
		}

		if(response != null){
			response.close();
		}
	} catch(IOException e){
		e.printStackTrace();
	}
}
```

만약 POI 라이브러리를 사용하고 있다면 아래와 같이 보다 간략하게 사용할 수 있습니다

```
CloseableHttpResponse httpclient = null;
CloseableHttpClient response = null;

try{
	...생략
} catch (Exception e){
	
} finally {
	IOUtils.closeQuietly(httpclient);
	IOUtils.closeQuietly(httpRes);
}
```

POI에서 제공하는 메서드로 closable을 구현한 클래스를  매개변수로 받습니다.
변수에 대해 null 체크를 하고 close()를 수행하고 에러 발생시 로그를 남깁니다.