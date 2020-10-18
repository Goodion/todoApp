<template>
  <div id="to-do-list">
    <h1>Список задач</h1>
    <p>Всего лайков: {{ overallLikes }}</p>
    <div class="list">
      <h3>Текущие</h3>
      <List v-bind:todos="uncompletedTodos" v-on:remove-todo="removeTodo" v-on:add-like="addLike"></List>
      <hr>
      <div class="todos-left">
        <span class="counter" v-if="countUncompleted() === 0">Невыполненных задач нет</span>
        <span class="counter" v-else-if="countUncompleted() === 1">Всего одна невыполненная задача</span>
        <span class="counter" v-else>Осталось сделать задач: {{ countUncompleted() }}</span>
        <transition name="fade">
          <img v-bind:src="require('@/assets/finger.jpg')" v-show="countUncompleted() === 0" alt="finger">
        </transition>
      </div>
    </div>
    <div class="form">
      <input v-model="toDoTitle" v-on:keyup.enter="addTodo">
      <button type="button" v-on:click="addTodo" v-bind:disabled="toDoTitle === ''">Добавить</button>
      <transition name="message-fade">
        <div class="warning" v-if="show">{{ myMessage }}</div>
      </transition>
    </div>
    <div class="list">
      <h3>Выполненные</h3>
      <List v-bind:todos="completedTodos" v-bind:isDone=true v-on:remove-todo="removeTodo" v-on:add-like="addLike"></List>
    </div>
  </div>
</template>

<script>
import List from "@/components/List";

export default {
  props: ['todos', 'myMessage', 'show', 'overallLikes'],
  computed: {
    uncompletedTodos () {
      return this.todos.filter(todo => !todo.isCompleted);
    },
    completedTodos () {
      return this.todos.filter(todo => todo.isCompleted);
    },
  },
  data () {
    return {
      toDoTitle: '',
    }
  },
  methods: {
    addTodo () {
      this.$emit('add-todo', this.toDoTitle);
      this.toDoTitle = '';
    },
    removeTodo (title) {
      this.$emit('remove-todo', title);
    },
    countUncompleted () {
      return this.uncompletedTodos.length;
    },
    addLike(args) {
      this.$emit('add-like', args);
    },
  },
  components: {
    List
  }
}
</script>

<style>
h1, h3 {
  text-align: center;
}
.list {
  padding: 20px;
  border: 1px solid #ccc;
  margin: 20px 0;
}
.form {
  margin: 20px 0;
}
.todos-left {
  margin-top: 15px;
  font-style: italic;
  color: gray;
}
.warning {
  padding: 7px 0;
  color: red;
}
img {
  display: block;
  margin: 50px auto;
  width: 150px;
}
.message-fade-enter-active, .message-fade-leave-active {
  transition: opacity .5s;
}
.message-fade-enter, .message-fade-leave-to {
  opacity: 0;
}
.fade-enter-active {
  animation: bounce-in 1s;
  transition: all .3s ease;
}
.fade-leave-active {
  animation: rotation 1s;
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.fade-enter, .fade-leave-to {
  transform: translateY(50px);
  opacity: 0;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes rotation {
  0% {
    transform:rotate(0deg);
  }
  100% {
  transform:rotate(180deg);
  }
}
</style>