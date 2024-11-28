<template>
  Created date
  <VueDatePicker v-model="date" range></VueDatePicker>
</template>

<script setup>
import { ref } from 'vue'
import VueDatePicker from '@vuepic/vue-datepicker'
import '@vuepic/vue-datepicker/dist/main.css'

const date = ref()
</script>
