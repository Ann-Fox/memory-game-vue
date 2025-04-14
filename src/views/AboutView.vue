<template>
  <HeaderComponent></HeaderComponent>
  <TransitionGroup tag="section" class="game-board" name="shuffle-card">
    <Card v-for="card in cardList" :key="`${card.value}-${card.variant}`" :value="card.value" :visible="card.visible"
      @select-card="flipCard" :position="card.position" :matched="card.matched"></Card>
  </TransitionGroup>
  <h2 class="status">{{ status }}</h2>
  <button v-if="newPlayer" @click="startGame" class="button-restart">
    <img src="../../public/images/play.svg"/> Start game</button>
  <button v-else @click="restartGame" class="button-restart">
    <img src="../../public/images/restart.svg"/> Restart game</button>
</template>

<script setup>
import _ from 'lodash'
import Card from "../components/Card.vue"
import { computed, ref, watch } from 'vue'
import { launchConfetti } from '../utilite/confetti'
import HeaderComponent from '@/components/HeaderComponent.vue';

const cardList = ref([]);
const userSelection = ref([])
const newPlayer = ref(true)

const startGame = ()=> {
newPlayer.value = false

restartGame()
}

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
// const shuffleCards = () => {
//   // cardList.value = _.shuffle(cardList.value)
// }

// Метод перезапуска игры
const restartGame = () => {
  cardList.value = _.shuffle(cardList.value)

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
    variant: 1,
    visible: false,
    position: null,
    matched: false
  })

  cardList.value.push({
    value: item,
    variant: 2,
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

watch(remainingPairs, currentValue => {
  if (currentValue === 0) {
    launchConfetti()
  }
})

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

.status {
  font-family: "Titillium Web", sans-serif;
}

.button-restart {
  background-color: orange;
  color: white;
  padding: 0.70rem 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  border: 2px solid orange;
  border-radius: 8px;
  font-weight: bold;
  font-family: "Titillium Web", sans-serif;
font-size: 1.1rem;
cursor: pointer;

}

.button-restart:hover {
  background-color: transparent;
  border: 2px solid orange;

}
.button-restart img {
  padding-right: 10px;
}


.shuffle-card-move {
  transition: transform 0.8s ease-in;
}
</style>
