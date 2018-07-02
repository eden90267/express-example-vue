<template>
  <div class="todolist">
    <header class="w3-bar w3-xlarge w3-blue">
      <div class="w3-bar-item">
        A Vue Simple Todo List
      </div>
    </header>

    <section class="w3-container w3-border w3-border-grey w3-center w3-padding">
      <to-do-form @addedTodo="addTodo"/>
    </section>

    <section class="w3-container w3-border w3-border-grey">
      <h2>ToDo´s</h2>
      <to-do 
      v-for="(todo, index) in uncompletedTodos" 
      :key="index"
      :todo="todo"
      :index="index"/>
    </section>

    <section class="w3-container w3-border w3-border-grey">
      <h2>Done</h2>
      <to-do 
      v-for="(todo, index) in completedTodos" 
      :key="index"
      :todo="todo"
      :index="index"/>
    </section>

  </div>
</template>

<script>
import ToDoForm from './ToDoForm'
import ToDo from './ToDo'

export default {
  components: {
    ToDoForm,
    ToDo
  },
  data () {
    return {
      todos: []
    }
  },
  created () {

    this.init();  
  },
  methods: {
    addTodo (todo) {
      let data = {
        title: todo,
        completed: false
      }
      let todos = this.todos;
      fetch('http://localhost:3000/api/users/spooky/tasks/create', {
        method: 'post',
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(data)
      }).then(function(response) {
        return response.json();
      }).then(function(data) {
        todos.push({
          title: todo,
          completed: false
        })
        
      });      
    },
    init () {
      let todos = this.todos;
      fetch("http://localhost:3000/api/users/spooky/tasks").then(function (response) {
            return response.json();
      }).then(function (result) {
          
          result.tasks.forEach((task) => {
            todos.push(task);
          });
      }); 
      
    }
  },
  computed: {
    completedTodos () {
      return this.todos.filter(todo => todo.completed);
    },
    uncompletedTodos () {
      return this.todos.filter(todo => !todo.completed);
    }
  }
}
</script>

<style scoped>
.todolist {
  width: 100%;
  height: 100%;
}
</style>
