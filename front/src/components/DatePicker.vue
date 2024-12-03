<template>
  Created date
  <VueDatePicker
    range
    :model-value="date"
    @update:model-value="handleDate"
    :timezone="tz"
  ></VueDatePicker>
</template>

<script setup>
// :timezone="'Australia/Melbourne'"
import { ref, defineEmits, computed } from 'vue'
import VueDatePicker from '@vuepic/vue-datepicker'
import '@vuepic/vue-datepicker/dist/main.css'

const date = ref()
const emit = defineEmits(['update-date'])
const handleDate = (modelData) => {
  date.value = modelData
  emit('update-date', date.value)
}

const tz = computed(() => {
  return { timezone: 'Australia/Melbourne' }
})
</script>
