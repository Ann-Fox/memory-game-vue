<template>
  <h1>This is an game</h1>
  <section class="game-board">
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>

  </section>
</template>

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
