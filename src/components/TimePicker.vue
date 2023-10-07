<template>
  <div class="time-picker">
    <input class="text-input" type="text" v-model="str" @input="event => parseValue(event.target.value)">
    <div class="content-container">
      <div class="content">

        <div class="item" v-if="props.format != '12'">
          <div class="icon-div">
            <PhCaretUp :size="20" @click="changeHours(1)" />
          </div>
          <input type="number" v-model="time.hours" min="0" max="23">
          <div class="icon-div">
            <PhCaretDown :size="20" @click="changeHours(-1)" />
          </div>
        </div>

        <div class="item" v-if="props.format == '12'">
          <div class="icon-div">
            <PhCaretUp :size="20" @click="changeHours12(1)" />
          </div>
          <input type="number" v-model="time.hours" min="0" max="11">
          <div class="icon-div">
            <PhCaretDown :size="20" @click="changeHours12(-1)" />
          </div>
        </div>

        <div class="item">
          <span>:</span>
        </div>

        <div class="item">
          <div class="icon-div">
            <PhCaretUp :size="20" @click="changeMinutes(1)" />
          </div>
          <input type="number" v-model="time.minutes" min="0" max="59">
          <div class="icon-div">
            <PhCaretDown :size="20" @click="changeMinutes(-1)" />
          </div>
        </div>

        <div class="item">
          <span>:</span>
        </div>

        <div class="item">
          <div class="icon-div">
            <PhCaretUp :size="20" @click="changeSeconds(1)" />
          </div>
          <input type="number" v-model="time.seconds" min="0" max="59">
          <div class="icon-div">
            <PhCaretDown :size="20" @click="changeSeconds(-1)" />
          </div>
        </div>

        <div class="item" v-if="props.format == '12'">
          <span>:</span>
        </div>

        <div class="item" v-if="props.format == '12'">
          <div class="icon-div">
            <PhCaretUp :size="20" @click="isAm = !isAm" />
          </div>
          <button @click="isAm = !isAm">{{ ampm }}</button>
          <div class="icon-div">
            <PhCaretDown :size="20" @click="isAm = !isAm" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { PhCaretUp, PhCaretDown } from "@phosphor-icons/vue";
import { ref, computed, onMounted } from 'vue';

const props = defineProps({
  format: String
})

const time = ref({
  hours: ("0" + 0).slice(-2),
  minutes: ("0" + 0).slice(-2),
  seconds: ("0" + 0).slice(-2)
})

function changeHours(val) {
  if (+time.value.hours == 0 && val < 0) {
    return " "
  } else {
    time.value.hours = (+time.value.hours + val) % 24
  }
}

function changeHours12(val) {
  if (+time.value.hours == 0 && val < 0) {
    return " "
  } else {
    time.value.hours = (+time.value.hours + val) % 12
  }
}

function changeMinutes(val) {
  if (+time.value.minutes == 0 && val < 0) {
    return " "
  } else {
    time.value.minutes = (+time.value.minutes + val) % 60
  }
}

function changeSeconds(val) {
  if (+time.value.seconds == 0 && val < 0) {
    return " "
  } else {
    time.value.seconds = (+time.value.seconds + val) % 60
  }
}

let isAm = ref(false)
let ampm = computed(() => {
  return isAm.value ? 'am' : 'pm'
})

const str = computed(() => {
  return `${('0' + time.value.hours).slice(-2)}:${('0' + time.value.minutes).slice(-2)}:${('0' + time.value.seconds).slice(-2)}${props.format == '12' ? ' ' + ampm.value : ''}`
})

function parseValue(val) {
  let arr = val.split(' ')[0].split(':')
  if (arr.length == 3) {
    time.value.hours = arr[0]
    time.value.minutes = arr[1]
    time.value.seconds = arr[2]
  }

  if (val.split(' ')[1].toLowerCase() == 'am') {
    isAm.value = true
  } else if (val.split(' ')[1].toLowerCase() == 'pm') {
    isAm.value = false
  }
}

</script>

<style scoped>
.time-picker {
  @apply w-96;
}

.text-input {
  @apply border-2 w-full p-2 rounded-md outline-none caret-sky-500 focus:border-sky-500 focus:outline-sky-200 focus:outline-2 focus:outline-offset-0 text-lg font-medium text-[#666]
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
  @apply text-xl
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
</style>