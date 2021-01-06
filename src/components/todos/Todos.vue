<template>
  <div>
    <!-- 新增 -->
    <EditTodo
      v-model:todo-title="newTodo"
      @keyup.enter="addTodo"
      autofocus
      placeholder="新增今日待办"
      autocomplete="off"
    ></EditTodo>
    <!-- todo 的列表  -->
    <ul>
      <TodoItem
        v-for="todo in filterdTodos"
        :key="todo.id"
        :todo="todo"
        v-model:edited-todo="editedTodo"
        @remove-todo="removeTodo"
      >
      </TodoItem>
    </ul>
    <!-- 过滤 -->
    <Filter :items="filterItems" v-model="visibility"></Filter>
    <!-- 回退 -->
    <button @click="backToDash">回退</button>
  </div>
</template>

<script>
import { reactive, toRefs, watch } from "vue";
import TodoItem from "./TodoItem.vue";
import Filter from "./Filter.vue";
import { useTodos } from "./useTodos.js";
import { useFilter } from "./useFilter.js";
import { onBeforeRouteLeave, useRoute, useRouter } from "vue-router";

export default {
  setup() {
    const todoState = reactive({
      newTodo: "",
      editedTodo: null, //正在编辑的todo
    });
    const { todos, addTodo, removeTodo } = useTodos(todoState);
    const filterState = useFilter(todos);

    //获取路由器实例
    const router = useRouter();

    //route 是响应式对象，可监控其变化
    const route = useRoute();
    watch(
      () => route.query,
      (query) => {
        console.log(query);
      }
    );

    //守卫
    onBeforeRouteLeave((to, from) => {
      const answer = window.confirm("你确定要离开页面吗");
      if (!answer) {
        return false;
      }
    });

    return {
      ...toRefs(todoState),
      ...toRefs(filterState),
      addTodo,
      removeTodo,
      backToDash() {
        router.push("/");
      },
    };
  },
  components: {
    TodoItem,
    Filter,
  },
};
</script>

<style scoped></style>
