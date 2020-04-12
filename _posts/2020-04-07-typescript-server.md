---
title: "Typescript를 이용한 node server 만들기"
author: Sungwook Kim
categories: node
tag: [typescript, node]
---

### dependencies
```json
"dependencies": {
    "@hapi/joi": "^17.1.1",
    "body-parser": "^1.19.0",
    "class-transformer": "^0.2.3",
    "class-validator": "^0.10.2",
    "config": "^3.3.1",
    "continuation-local-storage": "^3.2.1",
    "cookie-parser": "~1.4.4",
    "cors": "^2.8.5",
    "debug": "~2.6.9",
    "express": "~4.16.1",
    "http-errors": "~1.6.3",
    "multer": "^1.4.2",
    "mysql2": "^2.1.0",
    "nunjucks": "^3.2.0",
    "reflect-metadata": "^0.1.13",
    "routing-controllers": "^0.8.0",
    "sequelize": "^5.21.5",
    "sequelize-typescript": "^1.1.0",
    "typescript": "^3.8.3"
  },
  "devDependencies": {
    "@types/body-parser": "^1.19.0",
    "@types/bluebird": "^3.5.30",
    "@types/chai": "^4.2.10",
    "@types/config": "0.0.36",
    "@types/continuation-local-storage": "^3.2.2",
    "@types/cookie-parser": "^1.4.2",
    "@types/cors": "^2.8.6",
    "@types/debug": "^4.1.5",
    "@types/express": "^4.17.3",
    "@types/hapi__joi": "^16.0.12",
    "@types/http-errors": "^1.6.3",
    "@types/mocha": "^7.0.2",
    "@types/morgan": "^1.9.0",
    "@types/multer": "^1.4.2",
    "@types/node": "^13.7.7",
    "@types/nunjucks": "^3.1.3",
    "@types/validator": "^10.11.3",
    "@typescript-eslint/eslint-plugin": "^2.22.0",
    "@typescript-eslint/parser": "^2.22.0",
    "chai": "^4.2.0",
    "eslint": "^6.8.0",
    "mocha": "^7.1.0",
    "sinon": "^9.0.0",
    "ts-node": "^8.6.2"
  }
```
### Project Structure
```
.
├── config
│   ├── cors.ts
│   ├── local.json
│   └── routingControllers.ts
├── controllers
│   └── UserControllers.ts
├── database
│   ├── Hobby.ts
│   └── User.ts
├── middlewares
│   ├── auth
│   └── error
├── persistence
│   └── sequelize.ts
├── public
│   ├── favicon.ico
│   └── stylesheets
├── repository
│   └── user
├── test
│   ├── transaction
│   └── user
└── views
│   ├── error.html
│   └── index.html
├── index.ts
├── app.ts
├── .eslintrc.js
└── tsconfig.json
```
- config  
server, database 등 관련 설정 파일
- controllers  
  controller
- database  
  DB table entity
- middlewares  
  - auth
    인증 관련 미들웨어 (eg, passport)
  - error
    error handler
- persistence
  db connection
- public
  static view asset
- repository
  repository ( orm handler )
- test
  test folder
- views
  static view page
- index.ts
  entry point
- app.ts
  config express
- tsconfig.json
  typescript config file

## 핵심 library
### routing-controllers
- 사용이유  
  1. router를 class로 관리할 수 있다.
  2. async처리시 try catch 불필요
  3. response data를 return 처리해서 필요한 data에 집중할 수 있다.  
    typescript활용 가능
  4. router import를 자동으로 처리해준다.
- 컨벤션  
  <ol type="a">
    <li> response 형식이 json이면 JsonController를 사용한다.
    <li> method명에 literal을 쓰길 권장한다.<br>
      controller method는 일반적으로 call할 경우가 없기 때문에 api에 대한 설명으로 literal을 활용할 수 있다.
  </ol>
- Q&A  
  **Q** middleware 사용은 어떻게 하나요?
  **A** userBefore decorator를 사용합니다.  
    multer 또한 마찬가지 입니다.

[자세한 내용은](https://github.com/typestack/routing-controllers)

### sequelize
- spec  
  version: 5.0
- 사용이유
  1. 기존에 사용하던 knex로는 원하는 비즈니스 data set 관리하기가 힘들었다.
  2. node server에서 가장 많이 사용하는 orm이다.
- wrapping library
  - sequelize 4.0시절 typescript구현을 위한 라이브러리
  - sequelize 5.0또한 typescript를 지원하나  
    entity를 class로 관리할 수 있는 이점이 있고
    relation관계 설정이 좀 더 편리하다.
- transaction config
  continuation local storage를 사용해서 transaction 전달 코드를 줄인다.  
  [자세한 내용](https://sequelize.org/v5/manual/transactions.html#automatically-pass-transactions-to-all-queries)
- migration config
  추가적인 리서치후 구현이 필요
[sequelize-typescript에 대한 자세한 내용](https://github.com/RobinBuschmann/sequelize-typescript#readme)

### @hapi/joi
joi가 deprecated되고 @hapi/joi로 이전  
scheme로 object를 검증할 수 있다.  
자세한 내용은 https://hapi.dev/module/joi/#example

## TODO
- authorization 전략을 짜서 구현하기
