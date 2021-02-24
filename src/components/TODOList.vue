<template>
  <div id="app">
    <p>Кол-во невыполненных часов: {{ undoneTasksHours }}</p>
    <p>Кол-во выполненных часов: {{ doneTasksHours }}</p>
    <p>Кол-во выполеннных задач: {{ doneTasksCount }} </p>
    <p>Кол-во невыполеннных задач: {{ undoneTasksCount }} </p>

    <div class="TaskForm">
      <input type="text" v-model="newTask.title">
      <input type="number" v-model="newTask.hours" min="0">
      <label>Статус:</label>
      <input type="checkbox" value="Статус" v-model="newTask.done">
      <input @click="addTask()" :disabled="newTask.title.length < 1" type="button" value="Добавить">
    </div>
    <input @click="tasks = []" type="button" value="Очистить задачи">
    <div class="task" v-for="(task) in tasks" v-bind:key="task.id">
      <input v-model="task.title" type="text">
      <input v-model="task.hours" type="number" min="0">
      <input v-model="task.done" type="checkbox">
      <button @click="deleteTask(task.id)">Delete</button>
    </div>

  </div>
</template>

<script>

export default {
  name: 'TODOList',
  data () {
    return {
      newTask: {
        title: '',
        hours: 0,
        done: false,
      },
      tasks: []
    }
  },
  methods: {
    addTask() {
      if(this.newTask.hours < 0) {
        this.newTask.hours = -this.newTask.hours;
      }
      this.tasks.push({
        id: +new Date(),
        title: this.newTask.title,
        hours: this.newTask.hours,
        done: this.newTask.done
      });
      this.newTask.title = '';
      this.newTask.hours = 0;
      this.newTask.done = false;
    },
    deleteTask(id) {
      this.tasks.splice(this.tasks.findIndex(task => task.id === id), 1)
    }
  },
  computed: {
    undoneTasksHours() {
      let time = 0;
      this.tasks.filter(task => task.done === false).forEach((task) => {
        time += Number(task.hours);
      })
      return time;
    },
    doneTasksHours() {
      let time = 0;
      this.tasks.filter(task => task.done === true).forEach((task) => {
        time += Number(task.hours);
      })
      return time;
    },
    doneTasksCount() {
      return this.tasks.filter(task => task.done === true).length;
    },
    undoneTasksCount() {
      return this.tasks.filter(task => task.done === false).length;
    }
  },
  watch: {
    hours(value) {
      value < 0 ? this.newTask.hours = -value : this.newTask.hours = value;
    }
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

input {
  border-radius: 5px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 5px;
}


.TaskForm {
  display: grid;
  width: 33%;
  margin: auto;
}
</style>