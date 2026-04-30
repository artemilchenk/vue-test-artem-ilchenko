<template>
  <div class="nav" @click="$emit('click')">
    <slot />
  </div>
</template>

<script setup>
defineEmits(["click"]);
</script>

<style scoped lang="scss">
.nav {
  transition:
    opacity 0.2s ease,
    scale 0.2s ease;

  &:hover {
    opacity: 0.8;
    scale: 1.2;
  }
}
</style>
