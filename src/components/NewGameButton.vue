<template>
     <button v-if="newPlayer" @click="startNewGame" class="button-restart">
    <img src="../../public/images/play.svg"/> Start game</button>
  <button v-else @click="startNewGame" class="button-restart">
    <img src="../../public/images/restart.svg"/> Restart game</button>
</template>

<script setup>
import { defineEmits } from 'vue';

const props = defineProps ({
    newPlayer:{
        type: Boolean,
        default: true
    }
})

const emit = defineEmits();

const startNewGame = () => {
    emit('start-new-game', {
    });
};
</script>

<style scoped>
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
</style>