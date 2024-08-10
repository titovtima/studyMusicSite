<template>
  <Navbar @click-menu="showMenu = !showMenu"/>
  <TheoryMenu :open="showMenu"/>
  <!-- <div class="p-8 inline-block"> -->
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
  if (toValue(isWideScreen)) {
    showMenu.value = true;
    menuWidth.value = 10;
  }
});
</script>
