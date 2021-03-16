<template lang="pug">
.face
  button(
    @click='resetGame'
  ) {{ mood }}
</template>

<script>
export default {
  props: {
    status: String
  },
  methods: {
    resetGame() {
      this.$emit('gameReset')
    }
  },
  data() {
    return {
      happy: '🙂',
      angry: '😡'
    }
  },
  computed: {
    mood() {
      let current = this.happy
      if (this.status === 'OVER') {
        current = this.angry
      }
      return current
    }
  }
}
</script>

<style scoped>
.face button {
  flex: 0 0 0;
  font-size: 2em;
  background: transparent;
}
</style>