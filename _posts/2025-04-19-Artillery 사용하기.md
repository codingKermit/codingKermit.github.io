---
title: Artillery 사용하기
date: 2025-04-19
categories:
  - Node.js
tags:
  - Node
  - Artillery
---
안녕하세요 🐸  

오늘은 Artillery 를 통해 부하 테스트 하는 법을 정리해보겠습니다.  
저는 CLI를 통해 테스트 하는 것을 기본으로 하여 작성했습니다.  

## 설치

```
npm i -D Artillery
```

프로덕션 환경에선 불필요하니 -D 옵션을 붙여줍시다.  
## 사용법

CLI 에서 사용할 수 있는 명령어는 아래와 같습니다.  
- run
- run-lambda
- run-fargate
- run-aci
- quick
여기서 사용할 것은 run 과 quick 두 가지 입니다.  

### quick
하나의 HTTP 엔드포인트를 테스트 할 때 사용합니다.  
사용 형식은 아래와 같습니다.  

```
npx artillery quick [옵션] [url]
```

옵션의 종류는 다음과 같습니다.  

| 옵션                | 설명              |
| ----------------- | --------------- |
| `--count` , `-c`  | 요청 사용자 수        |
| `--num` , `-n`    | 사용자별 요청 수       |
| `--output` , `-o` | 결과를 json 파일로 작성 |
| `--rate` , `-r`   | 초당 요청 수         |

### run
테스트 스크립트를 실행시킵니다.  
개발자가 직접 시나리오를 작성하는 방법입니다.  
사용 형식은 아래와 같습니다.  

```
npx artillery run [옵션] [시나리오 파일]
```

시나리오 파일은 yaml 혹은 json 으로 작성합니다.  

```yml
config:
  target: 'https://service-foo.acmecorp.digital' # default target
  phases:
    - arrivalRate: 50
      duration: 600
  environments:
    local-dev:
      target: 'http://localhost:8080'
      phases:
        - arrivalRate: 10
          duration: 60
    preprod:
      target: 'https://service-foo.preprod.acmecorp.digital'
scenarios:
  # Scenario definitions would go here.
```

---
참조 : [https://www.artillery.io/docs](https://www.artillery.io/docs)  