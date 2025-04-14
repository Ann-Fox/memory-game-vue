<template>
  <button v-if="newPlayer" @click="startNewGame" class="button">
    <div class="button-icon">
      <img src="../../public/images/play.svg" class="icon-play" />
      <img src="../../public/images/cute-ghost.svg" class="icon-ghost" />
    </div>
    Start game
  </button>
  <button v-else @click="startNewGame" class="button">
    <img src="../../public/images/restart.svg" /> Restart game</button>
</template>

<script setup>
import { defineEmits } from 'vue';

const props = defineProps({
  newPlayer: {
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
.button {
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

.button-icon {
  position: relative;
  width: 24px;
  height: 24px;
  top: 0;
  margin-right: 10px;
}

.icon-ghost,
.icon-play {
  position: absolute;
  left: 0;
}

.button .icon-ghost {
  opacity: 0;
}

.button:hover {
  background-color: transparent;
  border: 2px solid orange;
}

.button:hover .icon-ghost {
  opacity: 1;
}

.button:hover .icon-play {
  opacity: 0;
}


.button img {
  padding-right: 10px;
}
</style>