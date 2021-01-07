<template>
  <img alt="Vue logo" height="1000" src="./assets/logo.png" />
  <!-- <HelloWorld msg="Hello Vue 3.0 + Vite" /> -->
  <!-- <Todos></Todos> -->
  <router-view></router-view>
  <!-- <router-view v-slot="{ Component }">
    <keep-alive>
      <component :is="Component"></component>
    </keep-alive>
  </router-view> -->
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Todos from "./components/todos/Todos.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    Todos,
  },
};
</script>
