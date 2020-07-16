---
title: "가장 좋은 유효성 검증은 무엇일까?"
author: Sungwook Kim
categories: typescript
tag: [typescript, vue, composition-api]
---

### 요구사항
1. typescript 지원
2. json 검증
3. browser와 server 모두 지원
4. type과 validation schema를 따로 만들지 않는다.

4가지를 모두 완벽히 지원하는 메이저 validator가 없다는 것에 놀랐다.
1,2,3은 많이들 지원하는데
4번은 typescript를 붙일 수 있다는 것 뿐이지 schema가 자료구조의 interface가 될 수 없다.

다만 우회적인 방법으로 지원하고 있다.

1. cli를 이용한 code generating
나쁘지 않아 보이지만 dto하나 만들 때 마다 cli를 실행 시킬 것인가?
typescript-json-scheme가 이런 방식인데
swagger형식의 doc type이라 작성자의 실수를 ide에서 막기 힘들다.

### class-validator
1. typescript 지원
2. json 검증
  class-transformer로 class로 만든후 validate실행
3. browser와 server 모두 지원
4. type과 validation schema를 따로 만들지 않는다.
  validation용 class를 바로 dto type으로 이용할 수 있다.

- 단점<br/>
object와 class 구분 작명이 필요하다.
typescript는 class라는 type을 제공하지 않는다.
class와 object를 구분할 수 없다.
form에서 value저장객체로 class를 사용하는 것이 바람직한지 의문이다.
vue같은 경우 v-model에 class property를 넘길 때 문제점이 없는지
reactive객체로 지정해도 정상 작동하는 지 알아봐야 한다.


