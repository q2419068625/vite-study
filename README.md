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

