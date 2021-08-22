### jest와 chai의 표현력 차이<br>
tdd에서 좀 더 행위 중심적으로 테스트를 하는 것을 bdd라고 한다.<br>
jest도 expect의 method를 보면 bdd스타일을 가지고 있으나<br>
표현할 수 있는 가지수가 chai에 비하면 적다.<br>
toHaveProperty라는 method가 있는데<br>
object의 property가 정상적으로 들어왔는 지 테스트하려면 property마다 method를 써줘야 한다.<br>
chai는 어떤지 보자<br>
```javascript
expect({a: 1, b: 2}).to.not.have.any.keys('c', 'd');
```
any대신 all을 쓸 수도 있고 not을 뺄 수도 있고<br>
한문장으로 테스트를 할 수 있다.<br>
테스트는 하나만 테스트하는 것이 일반적이다.<br>
chai방식이 표현력만 보면 테스트에 더 적합하다.<br>
