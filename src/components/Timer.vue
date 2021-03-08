<template lang="pug">
.timer
  span 000
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  total: Number
})
</script>

<style scoped>
.timer {

}
</style>