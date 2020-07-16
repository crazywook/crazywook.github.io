```javascript
const Dep = {
  target: null
}

function trace(message) {
  if (true)
    console.log('[TRACE]' + message)
}

function defineReactive(obj, key, val) {

  const deps = []

  Object.defineProperty(obj, key, {
    get: function() {
      if (Dep.target && deps.indexOf(Dep.target) == -1) {
        deps.push(Dep.target)
        // trace(`add target to ${key}`)
      }
      return val;
    },
    set: function(newValue) {
      val = newValue;
      trace(`setting value of ${key} to ${newValue}`)
      deps.forEach(onDependencyUpdated => {
        onDependencyUpdated()
      })
    }
  });
}

function defineComputed(obj, key, computeFunc, updateCallback) {
  let onDependencyUpdated = function () {
    trace(`${JSON.stringify(obj)}'s ${key} is updated to {computeFunc()} ${obj[key]}`)
  }
  Object.defineProperty(obj, key, {
    get: function() {
      Dep.target = onDependencyUpdated
      console.log('before computed')
      let value = computeFunc()
      Dep.target = null
      return value
    },
    set: function() {
      // computed 속성은 set 기능이 존재하지 않는다.
    }
  });
}
const person = {}

function computedStatus() {
    // 실제로 실행된 결과를 반환하는 함수(kind)
    const status = person.age < 18 ? "minor" : "adult"
    console.log("status computed called", status);
    return status
  }

defineComputed(
  person, // 대상 객체
  "status", // computed 속성을 위한 객체 속성
  computedStatus,
  function(newValue) {
    console.log("status has changed to", newValue);
  }
);

defineReactive(person, 'age', 25)
console.log("The person's status is: ", person.status);
console.log('change age')
person.age = 17
// console.log(person.age) 
```
