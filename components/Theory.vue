<template>
  <Navbar @click-menu="showMenu = !showMenu"/>
  <!-- <TheoryMenu ref="menu" id="theory-menu" class="absolute bg-white bg-opacity-95 p-8" :class="{hidden: !showMenu}"/> -->
  <TheoryMenu ref="menu" id="theory-menu" class="absolute bg-white bg-opacity-95 p-8" :style="showMenu ? '' : 'display: var(--menu-show)'"/>
  <!-- <div id="main-block" class="p-8 inline-block" :style="{ marginLeft: menuWidth + 'rem'}"> -->
  <div id="main-block" class="p-8 inline-block">
    <slot></slot>
  </div>
</template>

<script setup lang="ts">
import Navbar from './Navbar.vue';
import TheoryMenu from './TheoryMenu.vue';

const showMenu = ref(false);
</script>

<style scoped>
#main-block {
  margin-left: 10rem;
}

* {
  --menu-show: block;
}

@media screen and (max-width: 800px) {
  * {
    --menu-show: none;
  }

  #main-block {
    margin-left: 0;
  }
}
</style>
