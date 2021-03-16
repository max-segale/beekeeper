<template lang="pug">
.timer
  span ⏱ {{ currentTime }}
</template>

<script>
export default {
  props: {
    status: String
  },
  methods: {
    secsToMins(secs) {
      const minutes = Math.floor(secs % 3600 / 60);
      const seconds = Math.floor(secs % 3600 % 60);
      return this.twoDigit(minutes) + ':' + this.twoDigit(seconds)
    },
    twoDigit(number) {
      return (number < 10 ? '0' : '') + number
    }
  },
  computed: {
    currentTime() {
      if (this.status === 'STARTED' && !this.started) {
        this.started = true
        this.secondsTick = setInterval(() => {
          this.seconds += 1
        }, 1000)
      } else if (this.status === 'OVER') {
        clearInterval(this.secondsTick)
      }
      return this.secsToMins(this.seconds)
    }
  },
  data() {
    return {
      started: false,
      seconds: 0,
      secondsTick: null
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