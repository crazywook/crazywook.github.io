---
title: "Vue의 v-model은 Sugar Syntax일 뿐이다."
author: Sungwook Kim
categories: vue
tag: [typescript, vue, composition-api]
---
### 문제상황
input element를 처리하는 Component를 만들 때 input value를 parent로 보내려고 한다.<br/>
event처리 하거나 callback function을 보낼 것이다.<br/>
Vue가 React와 차별 되는 two way binding을 못쓰는 것이 불편할 것이다.<br/>
prop으로 전해지는 값은 변경하지 않기를 권장한다. 변경하더라도 v-model로 넘겨주는 값이 primitive 타입이라면<br/>
변경해도 추적하지 못한다.
Component를 만들고 v-model로 값을 받아서 input에 two way binding하는 방법은 없을까?
### v-model 구현
v-model은 내부적으로 복잡하게 구현 된 것이 아니고<br/>
props로 value를 받아서 input 이벤트로 돌려주는 것을 축약한 것이다.<br/>
그러면 value를 넘기고 input 이벤트를 만들면 바로 구현이 될까?
props로 받은 value를 input에 v-model로 매칭 시키면 다음과 같은 경고 메세지가 나온다.

`
[Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "value"
`

여기서 두가지 해결책으로 나눠진다.
- solve1) v-model에 prop대신 computed를 바인딩<br/>
Vue Computed는 definedProperty의 get을 사용한다.
자연스럽게 set을 만들어주면 v-model의 input event를 받을 수 있다.<br/>

```javascript
<template>
  <input v-model="inputValue"
  />
</template>

<script>
import Vue from 'vue'

export default {
  name: 'SelectInput',
  props: {
    value: {},
  },
  computed: {
    inputValue: {
      get: function() {
        return this.value
      },
      set: function(value) {
        this.$emit('input', value)
      },
    },
  },
}
```

- solve2)
v-model대신 v-bind:value로 바인딩

```javascript
<template>
<input
  :value="value"
  @input="handleInput"
/>
</template>

<script>
export default {
  name: 'SelectionPerPage',
  props: {
    value: {},
  },
  data() {
    const vm = this
    return {
      handleInput({ target }) {
        vm.$emit('input', target.value)
      }
    }
  }
}
</script>
```
취향에 맞게 쓰면 되지만
solve2는 inheritAttrs: false 를 사용하기 위해 눈에 익혀둘 필요가 있다.
