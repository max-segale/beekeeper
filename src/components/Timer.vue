<template lang="pug">
.timer
  span {{time}}
</template>

<script>
export default {
  data() {
    return {
      time: 0
    }
  },
  mounted() {
    setInterval(() => {
      this.time += 1
    }, 1000)
  }
}
</script>

<style scoped>
.timer {
  flex: 1 0 0;
  text-align: right;
}
</style>