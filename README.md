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
​```html
<template>
  <li>Hello</li>
  <li>Vue</li>
  <li>Devs!</li>
</template>
​```
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

