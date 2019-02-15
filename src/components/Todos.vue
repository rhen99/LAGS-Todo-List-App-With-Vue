<template>
  <div>
    <transition-group
      name="todo-item"
      tag="div"
      leave-active-class="animated fadeOutDown"
      :duration="300"
    >
      <div v-bind:key="todo.id" v-for="todo in todos">
        <TodoItem v-bind:todo="todo" v-on:del-todo="$emit('del-todo', todo.id)"/>
      </div>
    </transition-group>
  </div>
</template>
<script>
import TodoItem from "./TodoItem";
export default {
  name: "Todos",
  props: ["todos"],
  components: {
    TodoItem
  }
};
</script>

