# vite-study

# Teleport

创建一个包含全屏模式的组件。

`to` - `string`。需要 prop，必须是有效的查询选择器或 HTMLElement (如果在浏览器环境中使用)。指定将在其中移动 `<teleport>` 内容的目标元素

```html
<!-- 正确 -->
<teleport to="#some-id" />
<teleport to=".some-class" />
<teleport to="[data-teleport]" />

<!-- 错误 -->
<teleport to="h1" />
<teleport to="some-string" />
```

`disabled` - `boolean`。此可选属性可用于禁用 `` 的功能，这意味着其插槽内容将不会移动到任何位置，而是在您在周围父组件中指定了 `` 的位置渲染。

```html
<teleport to="#popup" :disabled="displayVideoInline">
  <video src="./my-movie.mp4">
</teleport>
```

# Fragments

vue3中可以不止一个根组件

```html
<template>
  <li>Hello</li>
  <li>Vue</li>
  <li>Devs!</li>
</template>
```

# Emits

emits 可以是数组或对象，从组件触发自定义事件，emits 可以是简单的数组，或者对象作为替代，允许配置和事件验证。

vue3中组件发送的自定义事件需要定要在emits选项中

```html
<template>
  <div @click="$emit('click')">自定义事件</div>
</template>
<script>
export default {
    emits:['click']
}
</script>
```

# createRenderer

Vue3.0中支持 *自定义渲染器* (Renderer):这个 API 可以用来创建自定义的渲染器, （在以往像weex和mpvue，需要通过fork源码的方式进行扩展）

### Global API 改为应用程序实例调用

vue3中使用createApp返回app实例，由它暴露一系列全局api

```html
import { createApp } from 'vue'
const app = createApp({})
	.component('comp', {render(){ return h('div','i am comp') }}
  	.mount('#app')

```

# model 选项和v-bind的sync修饰符被移除，统一为v-model参数形式

vue2中.sync和v-model功能有重叠，容易混淆，vue3做了统一。

```html
<div id="app">
  <h3>{{data}}</h3>    
  <comp v-model="data"></comp>
</div>
```

```js
app.component('comp', {
  template: `
    <div @click="$emit('update:modelValue', 'new value')">
    	i am comp, {{modelValue}}
    </div>
	`,
  props: ['modelValue'],
})
```

# 渲染函数API修改

渲染函数变得更简单好用了，修改主要有以下几点：

不再传入h函数，需要我们手动导入；拍平的props结构。scopedSlots删掉了，统一到slots

```html
<!-- render api的变化 -->
  <RenderTest v-model:counter="counter">
    <template v-slot:default>xxxxx</template>
    <template v-slot:counter>counter.....</template>
  </RenderTest>
```



```js
mport {h} from 'vue'

components: {
    RenderTest: {
      props: {
        counter: {
          type: Number,
          default: 0,
        }
      },
      render() {
        return h("div", [
          h("div", { onClick: this.onClick }, [
            `i am RenderTest${this.counter}`,
            this.$slots.counter(),
            this.$slots.default(),
          ])
        ]);
      },
      methods: {
        onClick() {
          this.$emit("update:counter", this.counter + 1);
        }
      }
    }
  }
```

# 函数式组件

 1.函数式组件只能使用接收 `props` 和 `context` 的普通函数创建

 2.`functional` attribute 在单文件组件 (SFC) `<template>`已被移除

 3.`{ functional: true }` 选项在通过函数创建组件已被移除

```js
import { h } from 'vue'

const DynamicHeading = (props, context) => {
  return h(`h${props.level}`, context.attrs, context.slots)
}

DynamicHeading.props = ['level']

export default DynamicHeading
```

# 异步组件要求使用 `defineAsyncComponent` 方法创建

由于vue3中函数式组件必须定义为纯函数，异步组件定义时有如下变化

1. 必须明确使用 `defineAsyncComponent` 定义
2. `component` 选项重命名为 `loader`
3. Loader 函数本身不再接收 `resolve` 和 `reject` 参数，且必须返回一个 Promise

```js
import { defineAsyncComponent } from 'vue'
// 不带选项的异步组件
const asyncPage = defineAsyncComponent(() => import('./NextPage.vue'))

```

带配置的异步组件，loader选项是以前的component

```js
import { defineAsyncComponent } from 'vue'
import ErrorComponent from './components/ErrorComponent.vue'
import LoadingComponent from './components/LoadingComponent.vue'

// 带选项的异步组件
const asyncPageWithOptions = defineAsyncComponent({
  loader: () => import('./NextPage.vue'),
  delay: 200,
  timeout: 3000,
  errorComponent: ErrorComponent,
  loadingComponent: LoadingComponent
})
```

# 自定义指令API和组件保持一致

vue3中指令api和组件保持一致，具体表现在：

- bind → **beforeMount**
- inserted → **mounted**
- **beforeUpdate**: new! 元素自身更新前调用, 和组件生命周期钩子很像
- update → removed! 和updated基本相同，因此被移除之，使用updated代替。
- componentUpdated → **updated**
- **beforeUnmount** new! 和组件生命周期钩子相似, 元素将要被移除之前调用。
- unbind  →  **unmounted**

```js
const app = Vue.createApp({})

app.directive('highlight', {
  beforeMount(el, binding, vnode) {
    el.style.background = binding.value
  }
})
```

```html
<p v-highlight="yellow">Highlight this text bright yellow</p>
```

# $on,$off and $once 移除

上述3个方法被认为不应该由vue提供，因此被移除了，可以使用其他三方库实现。

```html
<script src="https://unpkg.com/mitt/dist/mitt.umd.js"></script>
```

```js
// 创建emitter
const emitter = mitt()

// 发送事件
emitter.emit('foo', 'foooooooo')

// 监听事件
emitter.on('foo', msg => console.log(msg))
```

# vue-Router 变化

### history 选项替代了mode选项

- history:   createWebHistory()
- hash:  createWebHashHistory()
- abstract: createMemoryHistory()

### base 选项移至createWebHistory等方法中

### 通配符*被移除

### isReady() 替代 onReady()

```js
router.push()
// before
router.onReady(onSuccess, onError)
// now
router.isReady().then(onSuccess).catch(onError)
```

### scrollBehavior 变化

x,y 变成 top, left