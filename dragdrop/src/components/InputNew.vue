<script setup>
import { onMounted, ref, reactive, defineProps } from "vue";
//const props = defineProps(["onNewItem"]);
const text = ref("");
const emits = defineEmits(["onNewItem"]);

function handleKeyDown(evt) {
  if (evt.key === "Enter") {
    emits("onNewItem", text);
    text.value = "";
  }
}
</script>

<template>
New todo: <input v-model="text" @keydown="handleKeyDown" /></template>

<style scoped>
input {
    width: 100%;
    box-sizing: border-box;
}
form {
    margin-bottom: 10px;
}
</style>
