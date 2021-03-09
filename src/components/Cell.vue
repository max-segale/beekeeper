<template lang="pug">
.cell
  button(
    :class='className'
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
    display: String
  },
  data() {
    return {
      className: 'covered',
      displayVal: ''
    }
  },
  methods: {
    handleClick() {
      if (this.className !== 'flagged') {
        if (this.isKill) {
          this.className = 'dead'
          //this.$emit('killCell', [this.row, this.cell])
        } else {
          this.className = 'uncovered'
          if (this.display === '') {
            //this.$emit('emptyCell', [this.row, this.cell])
          }
        }
        this.displayVal = this.display
      }
    },
    handleRightClick(event) {
      if (this.className !== 'uncovered' && this.className !== 'dead') {
        if (this.className === 'flagged') {
          this.className = 'covered'
          this.displayVal = ''
        } else {
          this.className = 'flagged'
          this.displayVal = '🚩'
        }
      }
      //event.preventDefault()
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
  border: 1px solid #FFFFFF;
  overflow: hidden;
}
button {
  width: 100%;
  height: 100%;
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
