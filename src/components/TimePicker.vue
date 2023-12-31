<template>
  <div class="time-picker" @click="open = true; model()" @click.stop>
    <div class="display">
      <input type="number" v-model="time.hours" min="0" max="23" v-if="props.format != '12'">
      <input type="number" v-model="time.hours" min="0" max="11" v-if="props.format == '12'">
      <span>:</span>
      <input type="number" v-model="time.minutes" min="0" max="59">
      <span v-if="props.seconds">:</span>
      <input type="number" v-model="time.seconds" min="0" max="59" v-if="props.seconds">
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

const emit = defineEmits(['result'])

const model = () => {
  emit('result', obj)
}

const props = defineProps({
  format: String,
  seconds: Boolean,
  min: {
    type: String,
    default: "0"
  },
  max: {
    type: String,
    default: "86400"
  },
})

const open = ref(false)

const time = ref({
  hours: ("0" + 0).slice(-2),
  minutes: ("0" + 0).slice(-2),
  seconds: ("0" + 0).slice(-2)
})

let isAm = ref(false)
let ampm = computed(() => {
  return isAm.value ? 'am' : 'pm'
})

if (props.min) {
  time.value.hours = ("0" + Math.floor((props.min % 86400) / 3600)).slice(-2);
  time.value.minutes = ("0" + Math.floor((props.min % 86400) % 3600 / 60)).slice(-2);
  time.value.seconds = ("0" + Math.floor((props.min % 86400) % 3600 % 60)).slice(-2);
}

window.addEventListener('click', () => {
  open.value = false
})

function convert() {
  return +time.value.hours * 3600 + +time.value.minutes * 60 + +time.value.seconds
}

function changeHours(val) {
  if (val < 0 && convert() > props.min) {
    if (+time.value.hours == 0 && val < 0) {
      return " "
    } else {
      time.value.hours = ("0" + ((+time.value.hours + val) % 24)).slice(-2)
    }
  } else if (val > 0 && convert() < props.max) {
    if (+time.value.hours == 0 && val < 0) {
      return " "
    } else {
      time.value.hours = ("0" + ((+time.value.hours + val) % 24)).slice(-2)
    }
  }
}

function changeHours12(val) {
  if (val < 0 && convert() > props.min) {
    if (+time.value.hours == 0 && val < 0) {
      return " "
    } else {
      time.value.hours = ("0" + ((+time.value.hours + val) % 12)).slice(-2)
    }
  } else if (val > 0 && convert() < props.max) {
    if (+time.value.hours == 0 && val < 0) {
      return " "
    } else {
      time.value.hours = ("0" + ((+time.value.hours + val) % 12)).slice(-2)
    }
  }
}

function changeMinutes(val) {
  if (val < 0 && convert() > props.min) {
    if (+time.value.minutes == 0 && val < 0) {
      return " "
    } else {
      time.value.minutes = ("0" + ((+time.value.minutes + val) % 60)).slice(-2)
    }
  } else if (val > 0 && convert() < props.max) {
    if (+time.value.minutes == 0 && val < 0) {
      return " "
    } else {
      time.value.minutes = ("0" + ((+time.value.minutes + val) % 60)).slice(-2)
    }
  }
}

function changeSeconds(val) {
  if (val < 0 && convert() > props.min) {
    if (+time.value.seconds == 0 && val < 0) {
      return " "
    } else {
      time.value.seconds = ("0" + ((+time.value.seconds + val) % 24)).slice(-2)
    }
  } else if (val > 0 && convert() < props.max) {
    if (+time.value.seconds == 0 && val < 0) {
      return " "
    } else {
      time.value.seconds = ("0" + ((+time.value.seconds + val) % 24)).slice(-2)
    }
  }
}

const obj = computed(() => {
  const temp = {
    hh: +("0" + time.value.hours).slice(-2),
    mm: +("0" + time.value.minutes).slice(-2),
  }

  if (props.seconds) {
    temp.ss = +("0" + time.value.seconds).slice(-2)
  }

  if (props.format == '12') {
    temp.ampm = `${ampm.value}`
  }

  return temp
})

</script>