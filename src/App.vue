<template>
  <div class="app">
    <h1>Image Carousel</h1>

    <Carousel
      :images="images"
      :selected="selected"
      @update:selected="selected = $event"
    />

    <div class="selected">
      <h2>Selected Images</h2>
      <div class="selected-list">
        <img
          v-for="url in selected"
          :key="url"
          :src="url"
          class="selected-item"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import Carousel from './components/Carousel.vue'

const images = ref([])
const selected = ref([])

function generateImages(count) {
  return Array.from(
    { length: count },
    (_, i) => `https://picsum.photos/seed/${i}/600/400`
  )
}

onMounted(() => {
  images.value = generateImages(20)
})
</script>

<style scoped lang="scss">
.app {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;

  h1 {
    margin-bottom: 16px;
  }
}

.selected {
  margin-top: 24px;

  &-list {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }

  &-item {
    width: 100px;
    height: 70px;
    object-fit: cover;
    border-radius: 8px;
    transition: transform 0.3s ease;

    &:hover {
      transform: scale(1.05);
    }
  }
}
</style>
