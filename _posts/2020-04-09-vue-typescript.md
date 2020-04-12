---
title: "Typescript를 이용한 vue 구조화하기"
author: Sungwook Kim
categories: vue
tag: [typescript, vue, composition-api]
---
## 이전 프로젝트와 크게 다른 부분
1. typescript 적용
2. vue-bootstrap 적용
3. jquery 제거
4. 비지니스 로직을 container로 분리

## 프로젝트 생성
vue-cli를 사용
vuex, bootstrap 추가

### 이외 추가 library
```
@fortawesome/fontawesome-svg-core"
@fortawesome/free-solid-svg-icons
@fortawesome/vue-fontawesome
vuex-module-decorators
reflect-metadata
@vue/composition-api
```
- vue-fontawesome  
  기존에 fontawesome을 CDN으로 가져오는데 용량이 14.54MB로  
  사용자가 다운 받아사용하기에 상당히 많은 data traffic을 만든다.  
  필요한 부분만 들여와 빌드하면 상당히 많은 부분을 줄일 수 있다.  
  [자세한 내용](https://github.com/FortAwesome/vue-fontawesome)

- vuex-module-decorators\w
  js vuex는 type적용을 하기가 매우 어렵다.\
  또한 action이벤트는 스트링으로 발생시켜야 해서 실수의 여지가 크다.
  vuex-module-decorators를 사용하면 vuex module를 class로 관리할 수 있다.
  action, mutation, getter등 호출시 type을 적용할 수 있다.\
  [자세한 내용](https://championswimmer.in/vuex-module-decorators/pages/overview.html)

- @vue/composition-api
  typescript 적용을 위한 library\
  vue.extend로 type을 적용하는 것은 상당히 고통스러운 일이다.\
  보통 vue-class-component로 type적용을 하지만\
  composition-api의 type적용이 더 뛰어나다.\
  예를 들어 jsx로 렌더링 할 경우 type적용을 할 수 있다.\
  또한 Vue3.0에서 권장하는 component형식으로\
  추후 Vue3 프로젝트를 할 경우 브릿지 역할을 할 수 있다.\
  현재 프로젝트에서는 type적용을 위한 것이 주요 목적이며\
  presentation component에 적용한다.\
  [Vue2.0 Composition API](https://github.com/vuejs/composition-api)\
  [Vue3.0 Composition API](https://vue-composition-api-rfc.netlify.com/)

- bootstrap-vue
  bootstrap3는 jquery와 강하게 결합되어 있어서\
  vue의 가상돔 방식과 작동박싱에 괴리감이 크다.\
  bootstrap4부터는 jquery를 걷어냈다.\
  npm설치시 jquery peer warning이 뜨지만 무시해도 된다.\
  [자세한 내용](https://bootstrap-vue.js.org/docs/components/)

## container와 component
Vue 권장 스타일은 container와 component를 폴더로 구분하지 않는다.\
파일 컨벤션 권장하지만 한계가 있고 작명에 소요되는 시간을 늘릴 것으로 예상된다.
### Container
  1. 비지니스 로직만을 다룬다.
  2. html을 다루지 않는다.\
    (하지만 실제로는 slot을 사용, 경우에 따라서 div태그를 생성한다.)
### Component
  1. composition-api를 사용한다.
  2. 상태를 가지지 않는다.\
    하지만 v-bind와 data는 reactive로 연결돼 있다.\
    form같은 경우 input value를 container와 연결 시키면\
    프로그램 복잡도가 불필요하게 늘어난다.\
    이런 부분은 유연하게 대처할 필요가 있다.\
    Container와 Component의 분리 이유에 집중한다.
  
## event emit과 callback props pattern

vue의 공식 convention은 event emit방식이다. component간의 통신을 event방식으로 처리하는 이유는 위에서 아래로 한 방향으로만 제어권이 주어지는 것을 추구하기 때문이다. 여기서 얻을 수 있는 효과는 component의 dependency를 끊어내서 재사용 성을 높일 수 있기 때문이다. 반면에 react에서는 내부 component에 넘겨줄 수 있는 props의 data type은 제한이 없다.
그런데 vue는 $children과 $parent를 이미 제공하고 있기 때문에 단방향 통신을 프로그램 철학이라고 이야기하기 곤란하다.
또한 event방식은 물리적으로 끊어져있는 것이지 실제로 event를 처리하는 곳에서는 종속될 수 밖에 없다. 결국 어느 관점에서 종속성을 보느냐에 따라 구현 방향이 달라진다.\
[이에대한 비교글을 많이 찾아 볼 수 있다.](https://vuejsdevelopers.com/2018/07/30/callback-props-vs-emitting-events/)

이번 프로젝트 특성에는 다음과 같은 이유로 callback props 방식이 더 어울린다고 판단한다.
1. 비지니스 로직이 우선 되어야 한다고 생각한다면 container가 component에 요구사항을 제시할 수 있어야 한다고 생각한다.
2. container와 component가 올바르게 통신하고 있는 지 event방식으로는 compilation에서 type check를 할 수가 없다.\
  template방식의 한계가 크다. jsx방식을 사용한다면 typesafety를 확보할 수 있다.

## vue style guide에 대해
[참조](https://vuejs.org/v2/style-guide/)

## scoped slot
### 사용이유
container의 spec을 구조화하기 위함

### 공식문서
[링크](https://vuejs.org/v2/guide/components-slots.html#Scoped-Slots)

## composition api에서 직접 slot rendering하기
### 사용이유
- `template`에서 `slot`을 쓰려면 추가적인 root element가 필요하다.\
  불필요한 element는 dom 작동을 느리게 하고 css를 깨트릴 수 있다.
  또한 tr같은 부모 종속인 element는 사용할 수 없다.
### 필수 참조
[setup method](https://composition-api.vuejs.org/api.html#setup)
### 렌더링 방식
setup함수 interface
```typescript
type SetupFunction<Props, RawBindings> = (this: void, props: Props, ctx: SetupContext) => RawBindings | (() => VNode | null);
```
두번째 오버로딩 리턴값인 **() => VNode** 을 이용하면 된다.
주의할 점은 VNode는 Array가 될 수 없다.
여러 Node를 렌더링하려면 VNode안에 VNode를 만들거나 createElement를 이용해야 한다.

SetupContext interface
```typescript
interface SetupContext {
    readonly attrs: Record<string, string>;
    readonly slots: {
        [key: string]: (...args: any[]) => VNode[];
    };
    readonly parent: ComponentInstance | null;
    readonly root: ComponentInstance;
    readonly listeners: {
        [key: string]: Function;
    };
    emit(event: string, ...args: any[]): void;
}
```

SetupContext에는 넘어온 slots들이 key&value 형태로 들어있다.
올바른 type에 맞쳐서 사용하면 다음과 같다.
```typescript
export default defineComponent({
  setup(props, context) {

    return () => context.slots.'슬롯네임'(전달할 데이타)[0]
  }
})
```

하지만 단일 슬롯밖에 그릴 수 없다는 단점이 있다.
여러 슬롯을 받으려면 root element로 래핑할 수 밖에 없다.
이부분을 대신 처리해주는 함수를 만들어 본다.
```typescript
import { VNode } from 'vue'
import { SetupContext, createElement } from '@vue/composition-api'

export interface SlotsPropsFactoryNamespace {
  [index: string]: (...arg: any | undefined) => {}
}

export const createSlotNodeFactory = <T extends SlotsPropsFactoryNamespace>(context: SetupContext) => (
  // slotProps를 Object로 받으면 store의 reactive가 깨진다.
  slotPropsFactoryNamespace: T,
  defaultElement?: VNode,
) =>
  (): VNode => {
    const keys = Object.keys(slotPropsFactoryNamespace)

    if (keys.length === 0) {
      return defaultElement || createElement('div', ['empty!'])
    }
    if (keys.length === 1) {
      const props = slotPropsFactoryNamespace[keys[0]]()
      return context.slots[keys[0]](props)[0]
    }

    const slotNames = Object.keys(context.slots)
    const children: VNode[]= keys.map(key => {

      const props = slotPropsFactoryNamespace[key]()

      const slotIndex = slotNames.indexOf(key)
      if (slotIndex > -1) {
        slotNames.splice(slotIndex, 1)
      }
      return context.slots[key](props)[0]
    })
    if (slotNames.length > 0) {
      console.warn(`slot[${slotNames.join(',')}] are not used`)
    }
    return createElement('div', children)
  }
```

createNodeSlotNodeFactory를 이용해 setup method를 다시 구현해 본다.
```typescript
export default defineComponent({

  setup(_props, context: SetupContext) {

    return createSlotNodeFactory<SlotsContextType>(context)(slotsContext)
  }
})
```
넘겨 할 props type을 SlotsContextType로 강제해보았다.