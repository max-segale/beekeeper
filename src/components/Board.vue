<template lang="pug">
.board
  .row(v-for='(rowArr, rowNum) in cellMatrix'
    :key='rowNum'
  )
    Cell(v-for='(cellObj, cellNum) in rowArr'
      :ref='el => { if (el) cells[rowNum + "-" + cellNum] = el }'
      :key='rowNum + "-" + cellNum'
      :row='rowNum'
      :cell='cellNum'
      :is-kill='cellObj.bool'
      :display='cellObj.string'
      :game-status='status'
      @uncoverEmpty='clearSurround($event)'
      @gameStarted='updateStatus'
    )
</template>

<script>
import { ref } from 'vue'
import Cell from './Cell.vue'
export default {
  components: {
    Cell
  },
  props: {
    status: String,
    numRows: Number,
    numCols: Number,
    numKill: Number
  },
  data(props) {
    let gameStatus = props.status
    return {
      gameStatus
    }
  },
  setup(props) {

    const initCellVals = { bool: false, string: '' }
    const cellMatrix = Array(props.numCols).fill(Array(props.numRows).fill(initCellVals))
    const surrCoords = [[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, -1], [1, 0], [1, 1]]
    const cells = ref({})

    function getSurroundCells(rowNum, cellNum) {
      const surrCells = []
      surrCoords.forEach((coords) => {
        const newRowNum = rowNum + coords[0]
        const newCellNum = cellNum + coords[1]
        if (cellMatrix[newRowNum] && cellMatrix[newRowNum][newCellNum]) {
          surrCells.push([newRowNum, newCellNum])
        }
      })
      return surrCells
    }

    function getSurroundCount(rowNum, cellNum) {
      let nearby = []
      let count = 0
      surrCoords.forEach((coords) => {
        const newRowNum = rowNum + coords[0]
        const newCellNum = cellNum + coords[1]
        if (cellMatrix[newRowNum] &&
            cellMatrix[newRowNum][newCellNum] &&
            cellMatrix[newRowNum][newCellNum].bool) {
          nearby.push(1)
        }
      })
      count = nearby.reduce((total, num) => {
        return total + num
      }, 0)
      return count
    }

    function changeCell(rowNum, cellNum, newVals) {
      const newRow = [...cellMatrix[rowNum]]
      newRow[cellNum] = newVals
      cellMatrix[rowNum] = newRow
    }

    function rndNum(max) {
      return Math.floor(Math.random() * max)
    }

    function addKills() {
      let killRem = props.numKill
      let rowNum = 0
      let cellNum = 0
      while (killRem > 0) {
        rowNum = rndNum(props.numRows - 1)
        cellNum = rndNum(props.numCols - 1)
        if (!cellMatrix[rowNum][cellNum].bool) {
          changeCell(rowNum, cellNum, {bool: true, string: '🐝'})
          killRem -= 1
        }
      }
    }

    function addNums() {
      let surrCount = 0
      cellMatrix.forEach((rowArr, rowNum) => {
        rowArr.forEach((cellObj, cellNum) => {
          if (!cellObj.bool) {
            surrCount = getSurroundCount(rowNum, cellNum)
            if (surrCount > 0) {
              changeCell(rowNum, cellNum, {bool: false, string: surrCount + ''})
            }
          }
        })
      })
    }

    addKills()
    addNums()

    return {
      cellMatrix, cells, getSurroundCells
    }
  },
  methods: {
    clearSurround(coords) {
      const rowNum = coords[0]
      const cellNum = coords[1]
      const surrCells = this.getSurroundCells(rowNum, cellNum)
      surrCells.forEach((coors) => {
        const rowNum = coors[0]
        const cellNum = coors[1]
        const thisCell = this.cells[rowNum + '-' + cellNum]
        if (thisCell.className === 'covered') {
          thisCell.uncover()
          if (thisCell.display === '') {
            this.clearSurround([rowNum, cellNum])
          }
        }
      })
    },
    updateStatus() {
      this.gameStatus = 'STARTED'
      this.$emit('gameStarted')
    }
  }
}
</script>

<style scoped>
.board {
  margin: 0 auto;
  display: flex;
  flex-flow: column nowrap;
  align-items: stretch;
  width: 600px;
  height: 600px;
}
.row {
  flex: 1 0 0;
  display: flex;
  flex-flow: row nowrap;
  align-items: stretch;
}
</style>
