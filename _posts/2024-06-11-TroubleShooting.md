---
title: this web application instance has been stopped already
date: 2024-06-11
categories:
  - Tomcat
  - Error
tags:
  - Tomcat
---
업무 중에 맞닥들인 갑작스런 에러

```
this web application instance has been stopped already
```

찾아보니 서버가 구동 중일 때에 파일 변경이 발생하여 서버가 재시작되면서 발생하는 에러라고 합니다.
톰캣 에러인거죠.

찾아본 결과 이 에러의 해결 방법으로 두 가지를 보자면

1. 작업으로 인해 파일에 수정 사항이 생겼을 때에 서버가 자동으로 재시작 되도록 하는 옵션을 false로 변경하기
2. 서버 재시작하기

저는 그냥 서버 재시작했습니다.

전 아직도 갑자기 에러가 뜨면 깜짝깜짝 놀라요. 
설마 운영 서버에서도 이러는거 아니야?! 하는 생각에 등줄기가 서늘해지거든요.
이제 1년 되어가는 신입은 모든게 조심스럽고 놀랍네요.