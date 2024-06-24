---
title: Http delete의 body에 데이터 담기
date: 2024-06-24
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

하지만 되는 방법을 찾아내서 해야하는 막내인 저는 열씸히 찾았습니다!

`CloseableHttpClient` , `CloseableHttpResponse`, `HttpEntityEnclosingRequestBase` 이렇게 세 가지를 이용하는 방법입니다.

