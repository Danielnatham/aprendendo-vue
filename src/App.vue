<script setup>
import { ref } from "vue";

import PageHeader from "./components/PageHeader.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

let showAddTask = ref(false);

let tasks = ref([
  {
    id: 1,
    text: "Consulta Oftalmo",
    day: "1 de Março as 12:30",
    reminder: true,
  },
  {
    id: 2,
    text: "Consulta Otorrino",
    day: "2 de Março as 13:10",
    reminder: true,
  },
  {
    id: 3,
    text: "Consulta Clinico Geral",
    day: "3 de Março as 14:20",
    reminder: false,
  },
]);

function deleteTask(id) {
  if (confirm("Tem certeza?")) {
    tasks.value = tasks.value.filter((task) => task.id !== id);
  }
}

function toggleReminder(id) {
  tasks.value = tasks.value.map((task) =>
    task.id === id ? { ...task, reminder: !task.reminder } : task
  );
}

function addTask(task) {
  tasks.value = [...tasks.value, task];
}

function toggleAddTask() {
  showAddTask.value = !showAddTask.value;
}
</script>

<template>
  <div class="container">
    <PageHeader
      @toggle-add-task="toggleAddTask"
      title="Lista de Tarefas"
      :showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
