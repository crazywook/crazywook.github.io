---
title: "multi state구현하려면?"
author: Sungwook Kim
categories: frontend
tag: [state]
---

## state를 공유해도 되는가?
객체지향 프로그램의 가장 큰 단점은 내가 모르는 곳에서 상태가 바뀔 수 있다는 점이다.
그래서 불변성을 중요하게 생각하는 개발자는 함수형 기법을 많이 도입한다.
프론트엔드에서 사용하는 redux나 vuex등은 immutable 특성을 가지고 있다.
하지만 상태 제어권을 여러곳에서 가지고 있다면 이런 의도가 많이 퇴색된다.