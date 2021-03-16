<template lang="pug">
header
  h1 Beekeeper
  h2 It's Minesweeper, but with bees
.bar
  Counter(
    :total='killsLeft'
  )
  Face(
    :status='gameStatus'
    @game-reset='resetGame'
  )
  Timer(
    :status='gameStatus'
  )
.game
  Board(
    :status='gameStatus'
    :num-rows='numRows',
    :num-cols='numCols',
    :num-kill='numKills',
    @updateStatus='updateStatus($event)'
    @updateCount='updateCount($event)'
  )
footer
  span Built by #[a(href='https://maxsegale.com/' target='_blank' rel='noopener') Max Segale]
</template>

<script>
import Counter from './components/Counter.vue'
import Face from './components/Face.vue'
import Timer from './components/Timer.vue'
import Board from  './components/Board.vue'
export default {
  components: {
    Counter, Face, Timer, Board
  },
  methods: {
    updateStatus(newStatus) {
      this.gameStatus = newStatus
    },
    updateCount(amount) {
      this.killsLeft += amount;
    },
    resetGame() {
      location.reload()
    }
  },
  data() {
    return {
      numRows: 16,
      numCols: 16,
      numKills: 40,
      killsLeft: 40,
      gameStatus: 'READY'
    }
  }
}
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
