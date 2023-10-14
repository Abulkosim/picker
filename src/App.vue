<template>
  <div class="m-5">
    <TimePicker format="12" seconds min="3722" max="36122" @result="modelData($event)" />
    <h1 class="text-xl my-4 text-[#666] font-semibold">{{ time }}</h1>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import TimePicker from "./components/TimePicker.vue"

const time = ref('')

function modelData(e) {
  time.value = e;
}

</script>