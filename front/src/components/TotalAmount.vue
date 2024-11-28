<script>
export default {
  props: {
    totalAmount: {
      type: Number,
      required: true,
    },
  },
}
</script>
<template>
  Total amount: <span class="bold">$ {{ totalAmount }} </span>
</template>
<style scoped>
.bold {
  font-weight: 600;
}
</style>
