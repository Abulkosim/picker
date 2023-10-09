<template>
  <div class="time-picker" @click="open = true" @click.stop>
    <div class="display">
      <input type="number" v-model="time.hours" min="0" max="23" v-if="props.format != '12'" @input="resize">
      <input type="number" v-model="time.hours" min="0" max="11" v-if="props.format == '12'" @input="resize">
      <span>:</span>
      <input type="number" v-model="time.minutes" min="0" max="59" @input="resize">
      <span v-if="props.seconds">:</span>
      <input type="number" v-model="time.seconds" min="0" max="59" @input="resize" v-if="props.seconds">
      <button @click="isAm = !isAm" v-if="props.format == '12'">{{ ampm }}</button>
    </div>

    <Transition>
      <div class="content-container" v-if="open" @click.stop>
        <div class="content">
          <div class="item" v-if="props.format != '12'">
            <div class="icon-div">
              <PhCaretUp :size="18" @click="changeHours(1)" />
            </div>
            <input type="number" v-model="time.hours" min="0" max="23">
            <div class="icon-div">
              <PhCaretDown :size="18" @click="changeHours(-1)" />
            </div>
          </div>

          <div class="item" v-if="props.format == '12'">
            <div class="icon-div">
              <PhCaretUp :size="18" @click="changeHours12(1)" />
            </div>
            <input type="number" v-model="time.hours" min="0" max="11">
            <div class="icon-div">
              <PhCaretDown :size="18" @click="changeHours12(-1)" />
            </div>
          </div>

          <div class="item">
            <span>:</span> 
          </div>

          <div class="item">
            <div class="icon-div">
              <PhCaretUp :size="18" @click="changeMinutes(1)" />
            </div>
            <input type="number" v-model="time.minutes" min="0" max="59">
            <div class="icon-div">
              <PhCaretDown :size="18" @click="changeMinutes(-1)" />
            </div>
          </div>

          <div class="item" v-if="props.seconds">
            <span>:</span>
          </div>

          <div class="item" v-if="props.seconds">
            <div class="icon-div">
              <PhCaretUp :size="18" @click="changeSeconds(1)" />
            </div>
            <input type="number" v-model="time.seconds" min="0" max="59">
            <div class="icon-div">
              <PhCaretDown :size="18" @click="changeSeconds(-1)" />
            </div>
          </div>

          <div class="item" v-if="props.format == '12'">
            <span>:</span>
          </div>

          <div class="item" v-if="props.format == '12'">
            <div class="icon-div">
              <PhCaretUp :size="18" @click="isAm = !isAm" />
            </div>
            <button @click="isAm = !isAm">{{ ampm }}</button>
            <div class="icon-div">
              <PhCaretDown :size="18" @click="isAm = !isAm" />
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { PhCaretUp, PhCaretDown } from "@phosphor-icons/vue";
import { ref, computed } from 'vue';

const props = defineProps({
  format: String,
  seconds: Boolean
})

const open = ref(false)
const time = ref({
  hours: ("0" + 0).slice(-2),
  minutes: ("0" + 0).slice(-2),
  seconds: ("0" + 0).slice(-2)
})

function changeHours(val) {
  if (+time.value.hours == 0 && val < 0) {
    return " "
  } else {
    time.value.hours = ("0" + ((+time.value.hours + val) % 24)).slice(-2)
  }
}

function changeHours12(val) {
  if (+time.value.hours == 0 && val < 0) {
    return " "
  } else {
    time.value.hours = ("0" + ((+time.value.hours + val) % 12)).slice(-2)
  }
}

function changeMinutes(val) {
  if (+time.value.minutes == 0 && val < 0) {
    return " "
  } else {
    time.value.minutes = ("0" + ((+time.value.minutes + val) % 60)).slice(-2)
  }
}

function changeSeconds(val) {
  if (+time.value.seconds == 0 && val < 0) {
    return " "
  } else {
    time.value.seconds = ("0" + ((+time.value.seconds + val) % 60)).slice(-2)
  }
}

let isAm = ref(false)
let ampm = computed(() => {
  return isAm.value ? 'am' : 'pm'
})

window.addEventListener('click', () => {
  open.value = false
})


</script>
<style scoped>
.time-picker {
  @apply w-96;
}

.text-input {
  @apply border-2 w-full p-2 rounded-md outline-none caret-[#666] focus:border-sky-500 focus:outline-sky-200 focus:outline-2 focus:outline-offset-0 text-lg font-medium text-[#666]
}

.display {
  @apply flex gap-0.5 border-2 w-full p-2 rounded-md text-lg font-medium text-[#666] select-none cursor-pointer
}

.display input {
  @apply border-none outline-none w-6 text-center;
}

.content-container {
  @apply w-full h-28 mt-1 border shadow-sm rounded-md flex items-center justify-center
}

.content {
  @apply flex gap-2 items-center select-none
}

.item {
  @apply flex flex-col items-center font-semibold text-[#666] w-6 justify-center
}

.item input {
  @apply border-none outline-none w-full text-center
}

.icon-div {
  @apply w-5 h-5 rounded-full flex items-center justify-center hover:bg-[#f3f3f3] cursor-pointer active:bg-[#eeeeee]
}

span {
  @apply text-xl mt-[-1.5px]
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type=number] {
  -moz-appearance: textfield;
}

button {
  @apply outline-none
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

</style>