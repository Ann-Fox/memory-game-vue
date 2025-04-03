<template>
  <h1>This is an game</h1>
  <section class="game-board">
    <Card v-for="(card, index) in cardList" :key="`cadr-${index}`" :value="card.value" :visible="card.visible"
      @select-card="flipCard" :position="card.position"></Card>
  </section>
</template>

<script setup>
import Card from "../components/Card.vue"
import {ref} from 'vue'

const cardList = ref([]);

for (let index = 0; index < 18; index++) {
  cardList.value.push({
    value: index,
    visible: false,
    position: index
  })
}

const flipCard = (payload) => {
  cardList.value[payload.position].visible = true
}
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
