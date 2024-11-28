<template>
  Created date
  <VueDatePicker range :model-value="date" @update:model-value="handleDate"></VueDatePicker>
</template>

<script setup>
import { ref, defineEmits } from 'vue'
import VueDatePicker from '@vuepic/vue-datepicker'
import '@vuepic/vue-datepicker/dist/main.css'

const date = ref()
const emit = defineEmits(['update-date'])
const handleDate = (modelData) => {
  date.value = modelData
  emit('update-date', date.value)
}
</script>
