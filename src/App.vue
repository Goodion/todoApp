<template>
  <div id="app">
    <div class="list">
      <ToDoList
          v-bind:todos="todos"
          v-bind:myMessage="myMessage"
          v-bind:show="show"
          v-bind:overallLikes="countLikes"
          v-on:add-todo="addTodo"
          v-on:remove-todo="removeTodo"
          v-on:add-like="addLike"
      />
    </div>
  </div>
</template>

<script>
import ToDoList from "@/components/ToDoList";

export default {
  name: 'App',

  data () {
    return {
      todos: [
        {title: 'Развернуть окружение в Codepen', isCompleted: false, likes: 2},
        {title: 'Пройти курс по Vue', isCompleted: false, likes: 4},
        {title: 'Сделать интернет-магазин на Vue', isCompleted: true, likes: 0},
      ],
      myMessage: '',
      show: false
    }
  },
  computed: {
    countLikes() {
      return this.todos.reduce((value, item) => {
        return value + item.likes;
      }, 0);
    }
  },
  methods: {
    addTodo(toDoTitle) {
      if (toDoTitle === '') {
        this.showMessage('Поле не может быть пустым');
        return;
      }
      if (this.todos.findIndex(todo => todo.title === toDoTitle) === -1) {
        this.todos.push({
          title: toDoTitle,
          isCompleted: false,
          likes: 0
        });
      } else {
        this.showMessage('Такая задача уже существует!');
      }
    },
    removeTodo(toDoTitle) {
      this.todos.splice(this.todos.findIndex(todo => todo.title === toDoTitle), 1);
    },
    showMessage(msg) {
      this.myMessage = msg;
      this.show = true;
      setTimeout( () => {
        this.show = false;
        this.myMessage = '';
      }, 1000);
    },
    addLike(args) {
      this.todos[this.todos.findIndex(todo => todo.title === args.title)].likes = args.counter;
    }
  },
  components: {
    ToDoList
  }
}
</script>

<style>
body {
  max-width: 600px;
  margin: 0 auto;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
