<template>
  <div id="app">
    <h1>TODOS</h1>

    <div class="add-task-wrapper">
      <input type="text" v-model="newTaskInput" @keydown.enter="addTask">
      <button @click="addTask">Add a Task!</button>
    </div>
    <div class="task" v-for="task in tasks" :key="task.id">
      <span>{{ task.name }}</span>
      <span class="delete">
        <button @click="removeTask(task.id)">Remove</button>
      </span>
    </div>
  </div>
</template>

<script>

const API_URL = "https://eu96hlrox0.execute-api.us-west-2.amazonaws.com/todos";

export default {
  name: 'App',
  data() {
    return {
      newTaskInput: "",
      tasks: [],
    }
  },
  async created() {
    const response = await fetch(API_URL)
    this.tasks = await response.json()
  },
  methods: {
    async addTask() {
      let newTask = {
        id: Math.random(),
        name: this.newTaskInput,
      };
      await fetch(API_URL, {
        method: "post",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newTask)
      })
      this.tasks.push(newTask);
      this.newTaskInput = '';
    },
    async removeTask(taskId) {
      await fetch(API_URL, {
        method: "delete",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ id: taskId })
      })
      this.tasks = this.tasks.filter(t => t.id !== taskId);
    },
  },
  components: {
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
