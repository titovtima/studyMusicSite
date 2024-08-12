<template>
  <nav class="px-8 pt-2 border-b border-black min-h-8">
    <span ref="menuButton" id="menuButton" @click="$emit('clickMenu')">меню</span>
    <NuxtLink to="/" class="float-right">главная</NuxtLink>
  </nav>
</template>

<script setup lang="ts">
defineEmits(['clickMenu']);

const route = useRoute();

const menuButton: any = ref(null);
onMounted(() => {
  if (toValue(isWideScreen) || !route.path.includes('/theory/'))
    menuButton.value.classList.add('hidden');
});
</script>

<style scoped>
#menuButton {
  @apply cursor-pointer;
}

@media screen and (min-width: 800px) { 
  #menuButton {
    display: none;
  }
}
</style>
