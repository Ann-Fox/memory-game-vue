<template>
 <h1 class="sr-only">Peek-a-Vue</h1>
<img src="../../public/images/peek-a-vue-title.png" class="title-main"/>
<section class="description">
  <p>Welcom to Peek-a-Vue</p>
  <p>A card matching game powered  by Vue.js 3!</p>
</section>
</template>

<style scoped>
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

.description {
  font-family: "Titillium Web", sans-serif;
  /* font-weight: 600;
  font-style: normal; */
}

.description p {
  font-size: 1.2rem;
}

.description p:last-child {
  margin-bottom: 30px;
}
</style>
