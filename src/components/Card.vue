<template>
    <div class="card" :class="flippedStyle" @click="selectCard">
        <div  class="card-face is-front"><img :src="`/images/${value}.png`" :alt="value"/>
            <img v-if="matched" src="../../public/images/checkmark.svg" class="icon-checkmark" />
            
        </div>
        <div class="card-face is-back"></div>
    </div>
</template>

<script setup>

import { defineEmits, computed } from 'vue';

const props = defineProps({
    matched: {
        type: Boolean,
        default: false
    },
    position: {
        type: Number,
        required: true
    },
    value: {
        type: String,
        required: true
    },
    visible: {
        type: Boolean,
        default: true
    },
});

const emit = defineEmits();

const flippedStyle = computed(()=> {
    if (props.visible) {
        return 'is-flipped'
    }
})

const selectCard = () => {
    // emit('select-card', {
    //     position: props.position,
    //     faceValue: props.value
    // });
    if (!props.matched) {
        emit('select-card', {
        position: props.position,
        faceValue: props.value
    });
    }
};
</script>

<style>
.card {
    position: relative;
    transition: 0.5s transform ease-in;
    transform-style: preserve-3d;
}

.card.is-flipped {
    transform: rotateY(180deg);
}

.card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    backface-visibility: hidden;

}

.card-face.is-front {
    background-image: url('../../public/images/card-bg.png');
    color: white;
    transform: rotateY(180deg);

}

.card-face.is-back {
    background-image: url('../../public/images/card-bg-empty.png');
    color: white;
}

.icon-checkmark {
    position: absolute;
    bottom: 7px;
    right: 7px;
}
</style>