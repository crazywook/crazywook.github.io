---
title: "Typescript를 이용한 vue 구조화하기"
author: Sungwook Kim
categories: vue
tag: [typescript, vue, composition-api]
---
### 불필요한 주석
```javascript
// Method to get the value for a field
getFormattedValue(item, field) {
  const key = field.key
  const formatter = this.getFieldFormatter(key)
  let value = get(item, key, null)
  if (isFunction(formatter)) {
    value = formatter(value, key, item)
  }
  return isUndefinedOrNull(value) ? '' : value
},
```

```javascript
getFormattedFieldValue(item, field) {
```
함수 호출하는 곳에서 구현된곳의 주석을 보지 않아도 된다.
