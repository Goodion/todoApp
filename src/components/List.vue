<template>
  <div>
    <div class="item" v-for="todo in todos" v-bind:key="todo.title">
      <span class="deleteTodo" v-on:click="removeTodo(todo.title)">&#10005;</span>
      <input type="checkbox" v-model="todo.isCompleted">
      <span v-bind:class="{done: isDone}">{{ todo.title }}</span>
      <span><like-button v-bind:likes="todo.likes" v-on:add-like="addLike(todo.title, $event)" /></span>
    </div>
  </div>
</template>

<script>
import LikeButton from "@/components/LikeButton";

export default {
  name: "List",
  components: {
    LikeButton
  },
  props: ['todos', 'isDone'],
  methods: {
    removeTodo(title) {
      this.$emit('remove-todo', title);
    },
    addLike(title, counter) {
      this.$emit('add-like', {counter: counter, title: title});
    }
  }
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