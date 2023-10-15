<template>
  <div class="m-5">
    <TimePicker format="12" seconds min="3722" max="36122" @result="modelData($event)" />
    <h1 class="value">{{ time }}</h1>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import TimePicker from "./components/TimePicker.vue"

const time = ref('')

function modelData(event) {
  time.value = event;
}
</script>