<script setup>
defineProps({
  text: String,
  color: String,
});

let emit = defineEmits(["btn-click"]);

function onClick() {
  emit("btn-click");
}
</script>

<template>
  <button @click="onClick()" class="btn" :style="{ background: color }">
    {{ text }}
  </button>
</template>
