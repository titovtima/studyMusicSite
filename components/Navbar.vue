<template>
  <nav class="px-8 pt-2 border-b border-black min-h-8">
    <span v-if="!isWideScreen" class="cursor-pointer" @click="$emit('clickMenu')">меню</span>
  </nav>
</template>

<script setup lang="ts">
defineEmits(['clickMenu']);
</script>
