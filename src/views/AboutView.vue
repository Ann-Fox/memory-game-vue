<template>
  <h1>This is an game</h1>
  <section class="game-board">
    <Card v-for="(card, index) in cardList" :key="`cadr-${index}`" :value="card.value" :visible="card.visible"
      @select-card="flipCard" :position="card.position" :matched="card.matched"></Card>
  </section>
  <h2>{{ status }}</h2>
</template>

<script setup>
import Card from "../components/Card.vue"
import { ref, watch } from 'vue'

const cardList = ref([]);
const userSelection = ref([])
const status = ref('')

for (let index = 0; index < 18; index++) {
  cardList.value.push({
    value: index,
    visible: false,
    position: index,
    matched: false
  })
}

const flipCard = payload => {
  cardList.value[payload.position].visible = true

  if (userSelection.value[0]) {
    userSelection.value[1] = payload
  } else {
    userSelection.value[0] = payload
  }
  return userSelection
}
console.log(userSelection);

watch(userSelection, currentValue => {
  console.log(currentValue);

  if (currentValue.length === 2) {
 const cardOne = currentValue[0];
 const cardTwo = currentValue[1];

 if (cardOne.faceValue === cardTwo.faceValue) {
  status.value = "Совпадение"

  cardList.value[cardOne.position].matched = true
  cardList.value[cardTwo.position].matched = true
 } else {
  status.value = "Несовпадение"

  cardList.value[cardOne.position].visible = false
  cardList.value[cardTwo.position].visible = false
 }
    


    userSelection.value.length = 0
  }
}, { deep: true })

</script>

<style>
.card {
  border: 5px solid #ccc;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(6, 50px);
  grid-template-rows: repeat(3, 50px);
  grid-column-gap: 20px;
  grid-row-gap: 20px;
  justify-content: center;
}
</style>
