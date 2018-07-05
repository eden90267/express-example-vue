<template>
  <div class="todo w3-container w3-center">
    <label>{{index + 1}}.- {{todo.title}}</label>
    <input class="w3-check" type="checkbox" v-model="todo.completed" @change="updateTodo">
    <hr>
  </div>
</template>

<script>
export default {
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
  },
  methods: {
    async updateTodo(e) {
      let id = this.todo.id;
      let completed = e.target.checked;
      let data = {
        completed
      }
      let response = await fetch(`http://localhost:3000/api/task/${id}`, {
        method: 'put',
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify(data)
      });
    }
  }
}
</script>

<style scoped>

</style>
