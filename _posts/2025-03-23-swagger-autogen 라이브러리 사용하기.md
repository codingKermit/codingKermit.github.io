---
title: express에 swagger 적용하기
date: 2025-03-23
categories:
  - Node.js
tags:
  - Node
  - Swagger
  - express
---
안녕하세요 🐸  

강의 중 스웨거 연동하기라는 과제를 해보면서 swagger-autogen 이라는 라이브러리를 사용해봤는데 이 내용을 정리합니다.  

## Swagger 란?
RESTful API 를 문서화하는 도구입니다.  
문서화된 API는 Swagger ui 를 통해 직접 테스트해보고 요청/응답의 형식과 HTTP STATUS 상태에 따른 응답 값의 형태를 확인할 수 있습니다.

## swagger 적용 하기
swagger-autogen 라이브러리를 사용하기에 앞서 express.js 에서 swagger를 적용하는 방법에 대해 먼저 정리하겠습니다.  
express.js에 swagger를 적용하기 위해서는 먼저 `swagger-ui-express` 라는 라이브러리가 필요합니다.  

> npm i swagger-ui-express

위 명령어를 통해 설치합니다.  

swagger를 적용하기 위해 세 가지 방법을 찾아봤는데요.  
아래의 세 가지 입니다.
1. swagger.json 만들어서 사용하기
2. swagger-jsdoc 사용하기
3. swagger-autogen 사용하기

### swagger.json 만들어서 사용하기
가장 기본적인 방법입니다.  
다른 라이브러리를 필요로 하지 않습니다.  
하지만 다른 라이브러리를 사용하는 것이 편하기 때문에 주로 사용하지는 않습니다.  

사용 예시는 아래와 같습니다.

```javascript
const express = require('express');
const app = express();
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger.json');

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

`swagger.json` 파일을 읽어 내용을 `/api-docs` 에서 swagger로 보여주는 예시입니다.  
여기서 사용된 `swagger.json` 파일의 샘플은 아래 링크에서 확인할 수 있습니다.  
[https://swagger.io/docs/specification/v3_0/basic-structure/](https://swagger.io/docs/specification/v3_0/basic-structure/)  
작성 방법도 같이 확인 가능합니다.  

### swagger-jsdoc 사용하기
가장 많이 사용되는 방법입니다.  
주석을 통해 생성되며 yaml 문법을 사용합니다.  
아래의 명령어를 통해 설치합니다.

> npm i swagger-jsdoc


아래는 주석 작성의 예시 입니다.  

```javascript
/**
 * @openapi
 * /:
 *   get:
 *     description: Welcome to swagger-jsdoc!
 *     responses:
 *       200:
 *         description: Returns a mysterious string.
 */
app.get('/', (req, res) => {
  res.send('Hello World!');
});
```

주석의 시작에 `@openapi` 혹은 `@swagger` 를 명시하면 `swagger-jsdoc` 이 해당 내용을 읽어서 swagger 문서화를 해줍니다.  
그리고 보다 자세한 내용 작성 방법은 아래 문서에서 확인할 수 있습니다.  
[https://swagger.io/docs/specification/v3_0/about/](https://swagger.io/docs/specification/v3_0/about/)  

사용하고자 하는 버전이 3.0인지 2.0인지 확인 후, 문서에서 yaml 문법의 작성 예시를 찾을 수 있습니다.

위처럼 하나의 엔드포인트에 대한 내용을 주석으로 작성 했다면 이를 문서화 하기 위해 `swagger-jsdoc` 을 사용합니다.  

아래는 `swagger-jsdoc` 을 사용한 예시입니다.  

```javascript
const swaggerJsdoc = require('swagger-jsdoc');
const express = require('express');
const app = express();
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger.json');

// 아래 options 변수를 통해 스웨거 기본 옵션을 설정
const options = {
  definition: {
    openapi: '3.0.0', 
    info: {
      title: 'Hello World',
      version: '1.0.0',
    },
  },
  apis: ['./src/routes*.js'], // 라우터 파일 경로를 배열로 전달
};

const openapiSpecification = swaggerJsdoc(options);

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(openapiSpecification));
```

`swagger-jsdoc` 을 사용하지 않았을 때에는 json 파일을 직접 만들어서 사용했지만 이제는 `swagger-jsdoc` 가 `options` 의 내용을 토대로 json 형식의 내용을 생성합니다.  

### swagger-autogen 사용하기
단순히 swagger 문서를 빠르게 생성할 때에는 가장 손쉬운 방법입니다.  
프로젝트의 엔드포인트를 읽어서 json 파일을 자동으로 생성해줍니다.  
장점과 단점이 명확하다고 느껴지는 방법이기도 합니다.  
아래의 명령어를 통해 설치합니다.  

> npm i swagger-autogen

이후 루트 경로에 `swagger.js` 파일을 생성합니다.  
그리고 파일의 기본 내용은 아래의 형식을 가집니다.  
각각의 옵션에 맞는 값을 적절히 입력합니다.  예시로 서버의 포트가 8001이라면 servers의 url에도 알맞은 포트 번호를 입력해야 합니다  
파일명은 임의로 swagger.js 로 생성했지만 아래 packages.json 에서 통일 시켜준다면 파일명은 달라도 괜찮습니다.  

```javascript
const swaggerAutogen = require('swagger-autogen')({openapi: '3.0.0'});

const doc = {
  info: {
    version: '',            // by default: '1.0.0'
    title: '',              // by default: 'REST API'
    description: ''         // by default: ''
  },
  servers: [
    {
      url: '',              // by default: 'http://localhost:3000'
      description: ''       // by default: ''
    },
    // { ... }
  ],
  tags: [                   // by default: empty Array
    {
      name: '',             // Tag name
      description: ''       // Tag description
    },
    // { ... }
  ],
  components: {}            // by default: empty object
};

const outputFile = './swagger-output.json';
const routes = ['./path/userRoutes.js', './path/bookRoutes.js']; // 라우터 경로

/* NOTE: If you are using the express Router, you must pass in the 'routes' only the 
root file where the route starts, such as index.js, app.js, routes.js, etc ... */

swaggerAutogen(outputFile, routes, doc);
```

이후 `package.json` 의 script에 다음의 내용을 추가합니다.
여기서 실행시키는 파일은 위에서 만든 swagger 설정 파일입니다.

```json
{
	// ...
	"scripts":{
		// ...
		"swagger":"node ./swagger.js"
	}
	//...
}
```

이후 아래의 명령어를 사용하면 json 파일이 생성됩니다.  

> npm run swagger

이제 생성된 json 파일을 `swagger-ui-express` 에서 읽도록 use 해줍니다.  

```javascript
const express = require('express');
const app = express();
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger-output.json');

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

자동 생성인 만큼 일부 부족한 부분이 많은데요 이런 부분은 주석을 통해 보완할 수 있습니다.  
아래 링크에서 주석 다는 방법을 확인할 수 있습니다.  
[https://swagger-autogen.github.io/docs/endpoints/](https://swagger-autogen.github.io/docs/endpoints/)  

`swagger-autogen` 의 사용 방법은 공식 문서에서 자세하고 이해하기 쉽게 나와 있습니다.  

제가 느낀 이 라이브러리의 장단점이 있었습니다.  
- 장점
	1. 손쉽게 swagger 문서화 적용 가능
- 단점
	- json 파일을 생성하는 과정이 필요
	- `swagger-jsdoc` 에 비해 레퍼런스가 적음

json 파일을 생성해야하는 과정은 script에서 prestart 로 사용하면 서비스 구동할 때 마다 별도의 처리 없이 수행될테니 이것은 괜찮아 보입니다.  
그렇지만 레퍼런스가 적다는 것은 아쉽네요  
세 가지 방법을 전부 테스트 해봤을 때, 저는 `swagger-jsdoc` 을 사용하는 방법이 제일 편했습니다.  

---
참조 : [https://www.npmjs.com/package/swagger-ui-express](https://www.npmjs.com/package/swagger-ui-express)  
참조 : [https://www.npmjs.com/package/swagger-jsdoc](https://www.npmjs.com/package/swagger-jsdoc)  
참조 : [https://swagger-autogen.github.io/docs](https://swagger-autogen.github.io/docs)  
참조 : [https://swagger.io/docs/](https://swagger.io/docs/)  