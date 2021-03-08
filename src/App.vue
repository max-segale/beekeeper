<template lang="pug">
.bar
  counter(
    :total='numKills'
  )
  face
  timer
board(
  :num-rows='numRows',
  :num-cols='numCols',
  :num-kill='numKills'
)
</template>

<script setup>
// This starter template is using Vue 3 experimental <script setup> SFCs
// Check out https://github.com/vuejs/rfcs/blob/script-setup-2/active-rfcs/0000-script-setup.md

import { reactive } from 'vue'

import Counter from './components/Counter.vue'
import Face from './components/Face.vue'
import Timer from './components/Timer.vue'
import Board from  './components/Board.vue'

const numRows = 16
const numCols = 16
const numKills = 40

const state = reactive({
  status: null
})
</script>

<style>
* {
  margin: 0;
  border: 0;
  padding: 0;
  outline: none;
  box-sizing: border-box;
}
html, body {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
  background: #FFFFFF;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.bar {
  display: flex;
  flex-flow: row nowrap;
  align-items: center;
  justify-content: space-between;
}
</style>
