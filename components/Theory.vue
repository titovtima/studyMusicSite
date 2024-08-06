<template>
  <Navbar @click-menu="showMenu = !showMenu"/>
  <TheoryMenu :open="showMenu" :width="menuWidth"/>
  <div class="p-8 inline-block" :style="{ marginLeft: menuWidth + 'rem'}">
    <slot></slot>
  </div>
</template>

<script setup lang="ts">
import Navbar from './Navbar.vue';
import TheoryMenu from './TheoryMenu.vue';

const showMenu = ref(false);
const menuWidth = ref(0);
onMounted(() => {
  if (toValue(isWideScreen))
    showMenu.value = true;
  watch(showMenu, () => {
    menuWidth.value = showMenu.value ? 10 : 0;
  }, {immediate: true});
});
</script>

<style>
p {
    margin: 1rem 0;
}

h1 {
    font-size: 1.5rem;
    font-weight: 500;
    margin: 1rem 0;
}

h2 {
    font-size: 1.4rem;
    font-weight: 400;
    margin: 1rem 0;
}

h3 {
    font-size: 1.3rem;
    font-weight: 300;
    margin: 1rem 0;
}
</style>
