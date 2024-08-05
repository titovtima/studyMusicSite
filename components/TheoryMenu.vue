<template>
  <div :class="[`m-8 w-[${width}rem] absolute`, open ? 'inline-block' : 'hidden']">
    <NuxtLink to="/theory/notes">Ноты</NuxtLink><br/>
    <NuxtLink to="/theory/intervals">Интервалы</NuxtLink><br/>
    <NuxtLink to="/theory/key">Тональность</NuxtLink><br/>
  </div>
</template>

<script setup lang="ts">
defineProps({
  width: Number,
  open: Boolean
});
</script>
