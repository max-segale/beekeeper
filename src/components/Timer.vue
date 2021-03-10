<template lang="pug">
.timer
  span {{ currentTime }}
</template>

<script>
export default {
  props: {
    status: String
  },
  computed: {
    currentTime() {
      if (this.status === 'STARTED' && !this.started) {
        this.started = true
        setInterval(() => {
          this.time += 1
        }, 1000)
      }
      return this.time
    }
  },
  data() {
    return {
      started: false,
      time: 0
    }
  }
}
</script>

<style scoped>
.timer {
  flex: 1 0 0;
  text-align: right;
}
</style>