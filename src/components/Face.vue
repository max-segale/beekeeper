<template lang="pug">
.face
  span {{face}}
</template>

<script>
export default {
  data() {
    return {
      face: '🙂'
    }
  }
}
</script>

<style scoped>
.face {
  flex: 0 0 0;
  font-size: 2em;
}
</style>