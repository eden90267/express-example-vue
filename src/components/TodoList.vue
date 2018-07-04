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
  async mounted () {

    let response = await fetch("http://localhost:3000/api/users/hellojs/tasks");
    let result = await response.json();
    
    result.tasks.forEach((task) => {
      this.todos.push(task);
    });

  },
  methods: {
    async addTodo (title) {
      let data = {
        title,
        completed: false
      }

      let response = await fetch('http://localhost:3000/api/users/hellojs/tasks/create', {
        method: 'post',
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify(data)
      });

      let result = await response.json();

      this.todos.push(result.task)
        
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
