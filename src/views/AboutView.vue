<template>
  <h1>This is an game</h1>
  <section class="game-board">
    <Card v-for="(card, index) in cardList" :key="`cadr-${index}`" :value="card.value" :visible="card.visible"
      @select-card="flipCard" :position="card.position" :matched="card.matched"></Card>
  </section>
  <h2>{{ status }}</h2>
  <button @click="restartGame">Перезапустить игру</button>
</template>

<script setup>
import _ from 'lodash'
import Card from "../components/Card.vue"
import { computed, ref, watch } from 'vue'

const cardList = ref([]);
const userSelection = ref([])

// Статус игры, если нет ни одной пары, игрок выиграл, иначе, показать количество оставшихся пар
const status = computed(() => {
  if (remainingPairs.value === 0) {
    return "Ты победил!"
  } else {
    return `Количество оставшихся пар: ${remainingPairs.value}`;
  }
})

// Расчет количесва пар карт
const remainingPairs = computed(() => {
  const remainingCards = cardList.value.filter(card => card.matched === false).length

  return remainingCards / 2;
})

// Метод перемешивания карт
const shuffleCards = () => {
  cardList.value = _.shuffle(cardList.value)
}

// Метод перезапуска игры
const restartGame = () => {
  shuffleCards()

  cardList.value = cardList.value.map((card, index) => {
    return {
      ...card,
      matched: false,
      position: index,
      visible: false
    }
  })
}

// Формирование колоды карт
const cardItems = [1, 2, 3, 4, 5, 6, 7, 8, 9]

cardItems.forEach(item => {
  cardList.value.push({
    value: item,
    visible: true,
    position: null,
    matched: false
  })

  cardList.value.push({
    value: item,
    visible: true,
    position: null,
    matched: false
  })
})

cardList.value = cardList.value.map((card, index) => {
  return {
    ...card,
    position: index
  }
})
// Создание массива карт
// for (let index = 0; index < 18; index++) {
//   cardList.value.push({
//     value: 8,
//     visible: false,
//     position: index,
//     matched: false
//   })
// }

const flipCard = payload => {
  cardList.value[payload.position].visible = true

  if (userSelection.value[0]) {
    if (userSelection.value[0].position === payload.position && userSelection.value[0].faceValue === payload.faceValue) {
      return userSelection;
    } else {
      userSelection.value[1] = payload
    }
  } else {
    userSelection.value[0] = payload
  }
}

console.log(userSelection);

watch(userSelection, currentValue => {
  console.log(currentValue);

  if (currentValue.length === 2) {
    const cardOne = currentValue[0];
    const cardTwo = currentValue[1];

    if (cardOne.faceValue === cardTwo.faceValue) {
      // status.value = "Совпадение"

      cardList.value[cardOne.position].matched = true
      cardList.value[cardTwo.position].matched = true
    } else {
      // status.value = "Несовпадение"
      setTimeout(() => {
        cardList.value[cardOne.position].visible = false
        cardList.value[cardTwo.position].visible = false
      }, 2000)

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
