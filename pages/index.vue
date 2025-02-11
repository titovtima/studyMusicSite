<template>
  <div class="flex flex-row">
    <div class="split">
      <h1>Теория</h1>
      <TheoryMenu/>
    </div>
    <div class="split"><div>
      <h1>Упражнения на слух</h1>
      <NuxtLink to="/guessDegree">Определить ступень</NuxtLink><br/>
      <a href="https://guessinterval.studymusiconline.ru/" target="_blank">Определить интервал</a>
    </div></div>
  </div>
  <div style="width: 100%;">
    <BuildChord />
  </div>
</template>

<script setup lang="ts">
useHead({ title: 'StudyMusic' });
</script>

<style scoped>
.split {
  @apply w-1/2 p-8;
}

@media screen and (max-width: 800px) {
  h1 {
    font-size: 1.1rem;
  } 

  .split {
    @apply p-4;
  }
}
</style>

<style>
h1 {
  font-size: 1.8rem;
  font-weight: 700;
  margin: 1rem 0;
}
</style>
