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
  </div>
</template>

<script>
import { reactive, toRefs,} from "vue";
import TodoItem from "./TodoItem.vue";
import Filter from "./Filter.vue";
import { useTodos } from "./useTodos.js";
import { useFilter } from "./useFilter.js";



export default {
  setup() {
    const todoState = reactive({
      newTodo: "",
      editedTodo: null, //正在编辑的todo
    });
    const { todos, addTodo, removeTodo } = useTodos(todoState);
    const filterState = useFilter(todos)

    return {
      ...toRefs(todoState),
      ...toRefs(filterState),
      addTodo,
      removeTodo,
    };
  },
  components: {
    TodoItem,
    Filter,
  },
};
</script>

<style scoped></style>
