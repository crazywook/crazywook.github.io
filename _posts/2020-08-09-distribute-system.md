---
title: "나는 언제쯤 대규모 서비스를 해볼 수 있을까"
author: Sungwook Kim
categories: server
# tag: [store]
---
## 대용량 서비스를 하고 싶다.
이제 개발 경력 3년 8개월
나름대로 열심히 공부했지만 경력의 2/3는 새로운 프로젝트 구현에 보냈다.
의류 구독 서비스를 했던 회사가 성공하길 바랐지만
자본의 한계로 잘 되지 못한 것이 너무 아쉬웠다.
회원은 5만 가까지 받았지만 실제 구독자는 500명을 넘지 못했다.
잘 되어서 구독자가 만 명 정도 되었으면 정말 좋았을 텐데 너무 아쉬웠다.

## 나름 최소한의 자원으로 운영
구독자가 100명이 넘지 않았을 때까지는 t2-micro로 서비스를 했다.
클라이언트는 앱이라 트래픽은 백엔드만 신경을 쓰면 됐다.
100명 정도는 충분히 버틸 거라고 생각을 했는데
100명이 안된 상태에서도 신상 출시에는 서버 cpu사용률이 90% 이상 넘어갔다.
우리가 고객에게 가장 많이 보내는 데이터는 옷 사진이다.
이부분은 아마존 클라우드프론트에 맡긴 상태지만
api서버의 트래픽은 생각보다 많이 튀었고 대부분 DB사용량으로 보였다.
신상 출고는 보통 1주에 한 번이지만 대여이다 보니
재고 상태를 실시간으로 항상 보여줘야 한다.
계속 리로드해서 다시 보는 고객들도 있을 것이라 캐시 시간을 그렇게 오래 둘 수는 없었다.
그래서 3초 정도 서버에서 들고 있기로 결정을 했다.
그럭저럭 버티긴 했으나 이후 사용자가 더 늘어나서 t2-medium으로 확장을 했다.

## 정렬 작업 최소화
상품 목록은 최신순, 대여순, 인기순의 정렬 방법을 가지고 있다.
최신순과 달리 대여순, 인기순은 통계를 낸 후 조인 과정을 거쳐야해서
데이터가 쌓일 수록 점점 느려질 수 밖에 없었다.
서비스 특성상 대여순이나 인기순이 즉각 즉각 반영될 필요는 없었기 때문에
하루에 한번씩 정렬별 배치를 돌려서 테이블에 json으로 저장했다 불러썼다.

## 병렬 프로그램
몇 가지 통계를 서로 비교할 경우
순차적으로 하나씩 부르지 말고 비동기로 부르고
모두 완료되면 그때 처리하는 방식을 쓴다면 호출 시간을 줄일 수 있다.

## 서버분리
로그인 서버와 실제 서비스가 같은 서버를 사용할 필요가 있을까?
대출 서비스를 만든다면 실제로 대출은 누군가에게 대출금을 지급하고 이자와 수수료를 계산하고
대출금 상환을 받으면 된다.
그 사람이 어디 살고 어떤 아이디와 비번으로 접속했는 지는 알 필요가 없다.
대출이 가능한 고객인지는 다른 서버에서 검증된 고객만 대출 실행서버로 보내 주면 된다.
대출이 가능한지 조회하는 것은 실제 대출 보다 훨씬 많이 일어날 것이다.
대출 가능 여부를 판단하는 서버는 더 많은 트래픽을 처리할 수 있도록하고
대출을 실행하는 서버는 좀 더 낮은 사양으로 구성한다면 트래픽에 유연하게 대처할 수 있고 비용을 줄 일 수 있다.

## MSA를 구현해 보고 싶다.
모노리스 프로젝트는 규모가 점점 커질수록 유지보수 비용이 점점 늘어난다.
한번 배포할 때마다 전체 시스템을 테스트해야 한다.
배포의 유연성을 가지면서도 필요한 곳에 컴퓨팅 파워를 집중해서 트래픽 처리를 잘하고 비용을 효율적으로 관리할 수 있다.
또한 새로운 기술을 접목할 때 작은 프로젝트에 먼저 적용해 볼 수 있다.
하지만 MSA 제대로 구현하는 건 쉬운 일이 아니다.
서버 간 호출 시 세션 관리도 쉽지 않아 보인다.
거기다 트랜잭션 처리를 할 생각을 하면 머리가 아파진다.
잘 모른 채로 어설프게 뛰어들다 보면 프로젝트를 완성도 못 해볼 것 같다.
하지만 규모가 큰 프로젝트에서는 필요하다고 생각해서 꼭 해보고 싶다.