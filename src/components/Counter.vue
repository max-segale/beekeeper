<template lang="pug">
.counter
  span {{props.total}} 
</template>

<script setup>
const props = defineProps({
  total: Number
})
</script>

<style scoped>
.counter {

}
</style>