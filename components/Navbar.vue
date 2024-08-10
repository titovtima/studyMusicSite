<template>
  <nav class="px-8 pt-2 border-b border-black min-h-8">
    <span ref="menuButton" class="cursor-pointer" @click="$emit('clickMenu')">меню</span>
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
