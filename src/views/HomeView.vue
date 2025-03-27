<script setup>
// import TheWelcome from '../components/TheWelcome.vue'
</script>

<template>
  <main>
    <div class="parrent"> 
      <img src="../assets/image/card-joker.png">
    </div>
    <h1>Игра 'Memory'</h1>
    <RouterLink to="/about">Начать игру!</RouterLink>
  </main>
</template>

<style lang="scss" scoped>
.parrent {
  width: 200px;
  // background: gray;
}

.parrent img {
  width: 100%;
  // mix-blend-mode: multiply;
}
</style>