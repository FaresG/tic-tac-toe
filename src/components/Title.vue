<script setup>
defineProps({
  name: {
    type: String,
    required: true,
    default: ''
  },
  styling: {
    type: String,
    default: ''
  }
})
</script>

<template>
  <h1
    :class="styling"
    class="text-3xl text-center"
  >{{ name }}</h1>
</template>

<style scoped>

</style>