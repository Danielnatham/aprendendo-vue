<script setup>
import { ref, onMounted } from "vue";

import PageHeader from "./components/PageHeader.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

let showAddTask = ref(false);
let tasks = ref({});

onMounted(async () => {
  tasks.value = await fetchTasks();
});

async function fetchTasks() {
  const res = await fetch("api/tasks");

  const data = await res.json();

  return data;
}

async function fetchTask(id) {
  const res = await fetch(`api/tasks/${id}`);

  const data = await res.json();

  return data;
}

async function deleteTask(id) {
  const res = await fetch(`api/tasks/${id}`, {
    method: "DELETE",
  });

  res.status == 200
    ? (tasks.value = tasks.value.filter((task) => task.id !== id))
    : alert("Error ao deletar Tarefa");
}

async function addTask(task) {
  const res = await fetch("api/tasks", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(task),
  });

  const data = await res.json();

  tasks.value = [...tasks.value, data];
}

async function toggleReminder(id) {
  const taskToChange = await fetchTask(id);

  const uptadedTask = { ...taskToChange, reminder: !taskToChange.reminder };

  const res = await fetch(`api/tasks/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(uptadedTask),
  });

  const data = await res.json();

  tasks.value = tasks.value.map((task) =>
    task.id === id ? { ...task, reminder: !data.reminder } : task
  );
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
