<script setup>
import Task from "./Task.vue";

defineProps({
  tasks: Array,
});

defineEmits(["delete-task", "toggle-reminder"]);
</script>

<template>
  <div v-for="task in tasks" :key="task.id">
    <Task
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @delete-task="$emit('delete-task', task.id)"
      :task="task"
    />
  </div>
</template>

