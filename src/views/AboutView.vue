<template>
  <h1 class="sr-only">Peek-a-Vue</h1>
<img src="../../public/images/peek-a-vue-title.png" class="title-main"/>
  <section class="game-board">
    <Card v-for="(card, index) in cardList" :key="`cadr-${index}`" :value="card.value" :visible="card.visible"
      @select-card="flipCard" :position="card.position" :matched="card.matched"></Card>
  </section>
  <h2>{{ status }}</h2>
  <button @click="restartGame" class="button-restart">
    <img src="../../public/images/restart.svg"/> Restart game</button>
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
const cardItems = ['bat', 'candy', 'cauldron','cupcake', 'ghost', 'moon', 'pumpkin','witch-hat', 'cat']

cardItems.forEach(item => {
  cardList.value.push({
    value: item,
    visible: false,
    position: null,
    matched: false
  })

  cardList.value.push({
    value: item,
    visible: false,
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
.game-board {
  display: grid;
  grid-template-columns: repeat(6, 120px);
  grid-template-rows: repeat(3, 120px);
  grid-column-gap: 20px;
  grid-row-gap: 20px;
  justify-content: center;
}
/** Если нужно, чтобы элемент был доступен только для скринридеров */
.sr-only {
  clip: rect(0 0 0 0);
  clip-path: inset(50%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}

.title-main {
  margin-bottom: 40px;
}

.button-restart {
  background-color: orange;
  color: white;
  padding: 0.75rem 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  border: none;
  border-radius: 4px;
  font-weight: bold;
}

.button-restart img {
  padding-right: 10px;
}
</style>
