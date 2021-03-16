<template lang="pug">
.cell(
  :class='gameStatus === "OVER" ? "disabled" : null'
)
  button(
    :class='className'
    :disabled='gameStatus === "OVER" ? true : false'
    @click='handleClick'
    @contextmenu.prevent='handleRightClick'
  ) {{displayVal}}
</template>

<script>
export default {
  props: {
    row: Number,
    cell: Number,
    isKill: Boolean,
    display: String,
    gameStatus: String
  },
  methods: {
    uncover() {
      this.className = 'uncovered'
      this.displayVal = this.display
    },
    handleClick() {
      if (this.className !== 'flagged') {
        if (this.isKill) {
          this.className = 'dead'
          this.displayVal = this.display
          this.$emit('gameOver')
        } else {
          this.uncover()
          if (this.display === '') {
            this.$emit('uncoverEmpty', [this.row, this.cell])
          }
          if (this.gameStatus) {
            this.$emit('gameStarted')
          }
        }
        
      }
    },
    handleRightClick(event) {
      if (this.className !== 'uncovered' && this.className !== 'dead') {
        if (this.className === 'flagged') {
          this.className = 'covered'
          this.displayVal = ''
          this.$emit('flagCount', -1)
        } else {
          this.className = 'flagged'
          this.displayVal = '🚩'
          this.$emit('flagCount', 1)
        }
      }
    }
  },
  data() {
    return {
      className: 'covered',
      displayVal: ''
    }
  }
}
</script>

<style scoped>
.cell {
  flex: 1 0 0;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgb(255, 255, 255);
  overflow: hidden;
}
.cell.disabled button.covered:hover {
  background: rgba(175, 175, 175);
}
button {
  width: 100%;
  height: 100%;
  color: rgb(0, 0, 0);
  background: transparent;
}
.button:disabled {
  color: unset;
}
.button:disabled:hover {
  background: unset;
}
.covered {
  background: rgba(175, 175, 175);
}
.covered:hover {
  background: rgba(150, 150, 150);
}
.covered:active {
  box-shadow: inset 0 0 10px 0 rgba(0, 0, 0, .5)
}
.flagged {
  background: rgb(255, 100, 0);
}
.uncovered, .dead {
  box-shadow: inset 0 0 5px 0 rgba(0, 0, 0, .5);
}
.uncovered {
  background: rgb(255, 200, 0);
}
.dead {
  background: rgb(255, 0, 0);
}
</style>
