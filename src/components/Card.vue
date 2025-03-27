<template>
    <div class="card">{{ value }}</div>
</template>

<script setup>
defineProps({
    value: {
        type: Number,
        required: true
    },
});

</script>

<style>
.card {
    border: 5px solid #ccc;
}
</style>