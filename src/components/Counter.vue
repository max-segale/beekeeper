<template lang="pug">
.counter
  span {{total}} 🐝
</template>

<script>
export default {
  props: {
    total: Number
  }
}
</script>

<style scoped>
.counter {
  flex: 1 0 0;
  text-align: left;
}
</style>