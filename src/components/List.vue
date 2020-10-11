<template>
  <span>
    <div class="item" v-for="todo in todos" v-bind:key="todo.title">
    <span class="deleteTodo" v-on:click="removeTodo(todo.title)">&#10005;</span>
    <input type="checkbox" v-model="todo.isCompleted">
    <span class="done">{{ todo.title }}</span>
  </div>
  </span>
</template>

<script>
export default {
  name: "List",
  props: ['todos'],
}
</script>

<style scoped>
.item {
  margin: 10px 0;
}
.done {
  text-decoration: line-through;
}
.deleteTodo {
  color: red;
  font-weight: bold;
  cursor: pointer;
}
</style>