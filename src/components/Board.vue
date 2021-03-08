<template lang="pug">
.board
  .row(v-for='(rowArr, rowNum) in cellMatrix')
    cell(v-for='(cellObj, cellNum) in rowArr'
      :row='rowNum'
      :cell='cellNum'
      :is-kill='cellObj.bool'
      :display='cellObj.string'
    )
</template>

<script setup>

import { defineProps, reactive } from 'vue'
import Cell from './Cell.vue'

const props = defineProps({
  numRows: Number,
  numCols: Number,
  numKill: Number
})

const cellVals = {
  bool: false,
  string: ''
}
const cellMatrix = Array(props.numCols).fill(Array(props.numRows).fill(cellVals))

function rndNum(max) {
  return Math.floor(Math.random() * max)
}

function changeCell(rowNum, cellNum, newVals) {
  const newRow = [...cellMatrix[rowNum]]
  newRow[cellNum] = newVals
  cellMatrix[rowNum] = newRow
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

function getSurroundCount(rowNum, cellNum) {
  const surrCoords = [[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, -1], [1, 0], [1, 1]]
  let nearby = []
  let count = 0
  surrCoords.forEach((coords) => {
    const rowDiff = coords[0]
    const cellDiff = coords[1]
    if (cellMatrix[rowNum + rowDiff]) {
      if (cellMatrix[rowNum + rowDiff][cellNum + cellDiff]) {
        if (cellMatrix[rowNum + rowDiff][cellNum + cellDiff].bool) {
          nearby.push(1)
        }
      }
    }
  })
  count = nearby.reduce((total, num) => {
    return total + num
  }, 0)
  return count
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

</script>

<style scoped>

.board {
  display: flex;
  flex-flow: column nowrap;
  align-items: stretch;
  width: 600px;
  height: 600px;
  border: 1px solid rgba(0, 0, 0, 0.5);
  background: red;
}

.row {
  flex: 1 0 0;
  display: flex;
  flex-flow: row nowrap;
  align-items: stretch;
}

</style>
