<template>
    <div class="card" @click="selectCard">
        <div v-if="visible" class="card-face is-front">{{ value }}</div>
        <div v-else class="card-face is-back">back</div>
    </div>
</template>

<script setup>

import { defineEmits } from 'vue';

const props = defineProps({
    position: {
        type: Number,
        required: true
    },
    value: {
        type: Number,
        required: true
    },
    visible: {
        type: Boolean,
        default: true
    },
});

const emit = defineEmits();

const selectCard = () => {
  emit('select-card', {
    position: props.position
  });
};
</script>

<style>
.card {
    position: relative;
    border: 5px solid #ccc;
}

.card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.card-face.is-front {
    background: red;
    color: white;
}

.card-face.is-back {
    background: blue;
    color: white;
}
</style>