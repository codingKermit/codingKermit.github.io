---
title: CloseableHttpClient, CloseableHttpResponse 사용 시 주의할 점
date: 2024-06-24
categories:
  - Java
tags:
  - BackEnd
  - Java
---
안녕하세요 🐸

CloseableHttpClient 클래스와 CloseableHttpResponse  클래스는 HTTP 통신을 이용할 때 쓰는 클래스 입니다.

이 두 객체들은 통신이 끝난 이후 `close()`를 수행해줘야 합니다.

수행하지 않는다면 메모리 누수가 발생합니다!

### 기본적인 방법
가장 기본적인 예시로는 아래처럼 할 수 있습니다
```java

CloseableHttpResponse httpclient = null;
CloseableHttpClient response = null;

try{
	//...생략
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

### POI 라이브러리 사용하기
만약 POI 라이브러리를 사용하고 있다면 아래와 같이 보다 간략하게 사용할 수 있습니다

```java

CloseableHttpResponse httpclient = null;
CloseableHttpClient response = null;

try{
	//...생략
} catch (Exception e){
	
} finally {
	IOUtils.closeQuietly(httpclient);
	IOUtils.closeQuietly(httpRes);
}
```

POI에서 제공하는 메서드로 Closable 인터페이스를 구현한 클래스를  매개변수로 받습니다.
변수에 대해 null 체크를 하고 close()를 수행하고 에러 발생시 로그를 남깁니다.

## try-with-resources 사용하기
자바 7버전부터 지원되는 기능으로 네트워크 연결, 파일 읽기 등의 연결을 자동으로 닫아줍니다.
AutoCloseable 인터페이스를 구현한 객체를 대상으로 사용할 수 있습니다.  
HTTP 통신에 주로 사용되는 CloseAbleHttpResponse와 CloseableHttpcClient는 Closeable을 구현했으며 Closeable의 부모 인터페이스로 AutoCloseable이 추가되었기 때문에 사용이 가능합니다.

```java

try(CloseableHttpResponse httpclient = HttpClients.createDefault()){
	String url = "https://sample.api.com"
	
	try(CloseableHttpResponse response = httpclient.execute(deleteWithBody)){
		//...생략
	}
}
```

가장 코드가 간결합니다.  
속한 집단의 코드 컨벤션에 따라 선호하는 방식은 달라질 수 있습니다. 하지만 되도록이면 try-with-resources를 사용하는 것이 가장 가독성이 좋고 코드도 간결하기 때문에 지향하는 것이 좋겠습니다.