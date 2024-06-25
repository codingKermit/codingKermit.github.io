---
title: Http delete의 body에 데이터 담기
date: 2024-06-25
categories:
  - Back End
  - Java
tags:
  - BackEnd
  - Java
---
안녕하세요!🐸

지난 작업 회사에서 HTTP DELETE인 API를 사용하는데 request body에 데이터를 담아 보내달라는 요청이 있었습니다.

제가 작업하는 환경은 자바 7 버전을 사용 중 입니다.

먼저 위키피디아의 정보에 따르면 기본적으로 HTTP DELETE는 BODY에 담는 것이 선택 사항이라고 하고 있습니다

![](/assets/img/screenshot/Pasted%20image%2020240624183638.png) 

하지만 위의 표가 참조하고 있는 [RFC 9110](https://datatracker.ietf.org/doc/html/rfc9110) 은 2022년 이므로 위의 표는 2022년을 기준으로 작성되었음을 알 수 있습니다.

자바 7버전 에서는 HTTP DELETE의 request body에 데이터를 담아 보내는 것을 명시적으로는 지원하지 않습니다.

결론부터 얘기하자면 `HttpEntityEnclosingRequestBase` 클래스를 상속받은 객체를 생성하는 것으로 문제를 해결할 수 있었습니다.

여기서 먼저 `HttpEntityEnclosingRequestBase` 와 `HttpRequestBase`의 차이점에 대해 알아볼 필요가 있습니다.


|        | HttpEntityEnclosingRequestBase |           HttpRequestBase            |
| :----: | :----------------------------: | :----------------------------------: |
| 라이브러리  |       Apache httpClient        |          Apache httpClient           |
| 엔티티 여부 |               O                |                  X                   |
|   상속   |     HttpRequestBase를 상속 받음     | AbstractExecutionAwareRequest를 상속 받음 |

<small>HttpEntityEnclosingRequestBase는 HttpRequestBase의 확장 버전 같은거구나!</small>

Apache httpClient는 HTTP 통신을 돕는 라이브러리입니다.   
GET, POST, PUT, DELETE 에 대응하는 클래스를 가지고 있으며 각각의 구현 클래스는 HttpEntityEnclosingRequestBase 혹은 HttpRequestBase를 상속 받습니다.  
그렇다면 위의 표에서 알 수 있듯이 엔티티를 사용해야하는 POST 요청은 HttpEntityEnclosingRequestBase를 상속 받았음을 유추할 수 있습니다.  
제가 회사에서 사용중인 Apache httpClient 라이브러리 버전은 4.3.6버전이며,  
해당 버전에서 상속 관계는 아래의 표와 같습니다.


| HttpEntityEnclosingRequestBase 상속 |                HttpRequestBase 상속                |
| :-------------------------------: | :----------------------------------------------: |
| HttpPatch<br>HttpPost<br>HttpPut  | HttpGet<br>HttpDelete<br>HttpOptions<br>HttpHead |

<small>Patch, Options, Head는 처음보네요</small>

여기서 알 수 있듯이 HTTP DELETE를 지원하는 HttpDelete 클래스는 HttpRequestBase를 상속 받았기 때문에 엔티티를 사용할 수 없습니다.

그렇다면 이제 우리는 HttpEntityEnclosingRequestBase 를 상속받아 구현한 객체를 통해 HTTP METHOD와 ENTITY를 사용할 수 있음을 알았습니다. 이제는 직접 구현 예시를 만들어보겠습니다.

```
public class HttpDeleteWithEntity extends HttpEntityEnclosingRequestBase{
	public static final String METHOD_NAME = "DELETE";

    public HttpDeleteWithEntity() {
        super();
    }

    public HttpDeleteWithEntity(final URI uri) {
        super();
        setURI(uri);
    }
	
    public HttpDeleteWithEntity(final String uri) {
        super();
        setURI(URI.create(uri));
    }
    
	@Override
	public String getMethod() {
		return METHOD_NAME;
	}
}
```

HttpPost의 구조와 동일하며 static 변수인 METHOD_NAME의 값이 DELETE 입니다.

이제 entity를 사용할 수 있으며 DELETE로 요청을 보낼 수 있는 클래스를 만들었습니다.

이렇게 만든 객체를 사용해 통신하는 예시입니다.

```
...생략

try(CloseableHttpClient httpClient = HttpClients.createDefault()){
	String url = "https://api.example.com" // 예시를 들기 위해 아무거나 적었습니다
	
	HttpDeleteWithEntity httpDelete = new HttpDeleteWithEntity(url);
	
	try(CloseableHttpResponse response = httpclient.execute(httpDelete)){
		String entity = EntityUtils.toString(response.getEntity());
		...생략
	}
}

```

