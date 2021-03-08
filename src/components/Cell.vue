<template lang="pug">
.cell
  button(
    :class='state.className'
    @click='handleClick'
    @contextmenu='handleRightClick'
  ) {{state.displayVal}}
</template>

<script setup>

import { defineProps, reactive, onMounted, onBeforeUpdate } from 'vue'

const props = defineProps({
  row: Number,
  cell: Number,
  isKill: Boolean,
  display: String
})

const state = reactive({
  className: null,
  displayVal: ''
})

function handleClick() {
  console.log($emit)
  if (state.className !== 'flagged') {
    if (props.isKill) {
      state.className = 'dead'
    } else {
      state.className = 'uncovered'
    }
    state.displayVal = props.display
  }
}

function handleRightClick(event) {
  if (state.className !== 'uncovered' && state.className !== 'dead') {
    if (state.className === 'flagged') {
      state.className = null
      state.displayVal = ''
    } else {
      state.className = 'flagged'
      state.displayVal = '🚩'
    }
  }
  event.preventDefault()
}

</script>

<style scoped>

.cell {
  flex: 1 0 0;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(0, 0, 0, 0.5);
  overflow: hidden;
}

button {
  width: 100%;
  height: 100%;
  background: grey;
}

button:active {
  box-shadow: inset 0 0 10px 0 rgba(0, 0, 0, .5)
}

button.flagged {
  background: orange;
}

button.uncovered {
  background: yellow;
}

button.dead {
  background: red;
}

</style>
