<template lang="pug">
header
  h1 Beekeeper
  h2 It's Minesweeper, but with bees
.bar
  Counter(
    :total='numKills'
  )
  Face
  Timer
.game
  Board(
    :num-rows='numRows',
    :num-cols='numCols',
    :num-kill='numKills'
  )
footer
  span Built by #[a(href='https://maxsegale.com/' target='_blank' rel='noopener') Max Segale]
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
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Courier New', Courier, monospace;
  font-size: 20px;
  font-weight: 700;
  text-align: center;
  color: #FFFFFF;
  background: #5f8337;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.wrapper {
  height: 100%;
  display: flex;
  flex-flow: column nowrap;
  align-items: stretch;
  justify-content: center;
}
header, footer {
  flex: 1 1 0;
  padding: .5rem 0;
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
}
header {
  justify-content: center;
}
footer {
  justify-content: flex-end;
  font-size: .8em;
}
a:link, a:visited {
  color: inherit;
  text-decoration: none;
}
a:hover, a:active {
  text-decoration: underline;
}
.bar {
  display: flex;
  flex-flow: row nowrap;
  align-items: baseline;
  justify-content: space-between;
}
.game {
  padding: 10px;
  background: #FFFFFF;
}
</style>
