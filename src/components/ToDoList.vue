<template>
  <div id="to-do-list">
    <h1>Список задач</h1>
    <div class="list">
      <h3>Текущие</h3>
      <div class="item" v-for="todo in uncompletedTodos" v-bind:key="todo.title">
        <span class="deleteTodo" v-on:click="removeTodo(todo.title)">X</span>
        <input type="checkbox" v-model="todo.isCompleted">
        {{ todo.title }}
      </div>
      <hr>
      <div class="todos-left">
        <span class="counter" v-if="countUncompleted() === 0">Невыполненных задач нет</span>
        <span class="counter" v-else-if="countUncompleted() === 1">Всего одна невыполненная задача</span>
        <span class="counter" v-else>Осталось сделать задач: {{ countUncompleted() }}</span>
        <transition name="fade">
          <img v-bind:src="require('@/assets/finger.jpg')" v-show="countUncompleted() === 0">
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
      <h3 >Выполненные</h3>
      <div class="item" v-for="todo in completedTodos" v-bind:key="todo.title">
        <span class="deleteTodo" v-on:click="removeTodo(todo.title)">X</span>
        <input type="checkbox" v-model="todo.isCompleted">
        <span class="done">{{ todo.title }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['todos', 'myMessage', 'show'],
  computed: {
    uncompletedTodos () {
      return this.todos.filter(todo => !todo.isCompleted)
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
.item {
  margin: 10px 0;
}
.done {
  text-decoration: line-through;
}
.form {
  margin: 20px 0;
}
.todos-left {
  margin-top: 15px;
  font-style: italic;
  color: gray;
}
.deleteTodo {
  color: red;
  font-weight: bold;
  cursor: pointer;
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