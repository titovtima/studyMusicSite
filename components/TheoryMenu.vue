<template>
  <div :class="[`p-8 w-[${width}rem] absolute bg-white bg-opacity-95`, open ? 'inline-block' : 'hidden']">
    <NuxtLink to="/theory/notes">Ноты</NuxtLink><br/>
    <NuxtLink to="/theory/intervals">Интервалы</NuxtLink><br/>
    <NuxtLink to="/theory/key">Тональность</NuxtLink><br/>
    <NuxtLink to="/theory/chords">Аккорды</NuxtLink><br/>
  </div>
</template>

<script setup lang="ts">
defineProps({
  width: Number,
  open: Boolean
});
</script>
