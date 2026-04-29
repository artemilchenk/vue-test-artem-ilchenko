<template>
  <div class="carousel">
    <button class="nav left" @click="prev">◀</button>

    <div class="viewport">
      <div
        class="track"
        :style="{ transform: `translateX(-${index * itemWidth}px)` }"
      >
        <div
          v-for="(img, i) in loopedImages"
          :key="i"
          class="item"
          :style="{ width: itemWidth + 'px' }"
        >
          <img
            :src="img"
            @click="toggle(img)"
            :class="['image', { selected: selected.includes(img) }]"
          />
        </div>
      </div>
    </div>

    <button class="nav right" @click="next">▶</button>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  images: Array,
  selected: Array
})

const emit = defineEmits(['update:selected'])

const index = ref(0)
const itemWidth = 220

const loopedImages = computed(() => [...props.images, ...props.images])

function prev() {
  index.value = index.value === 0 ? props.images.length - 1 : index.value - 1
}

function next() {
  index.value = (index.value + 1) % props.images.length
}

function toggle(img) {
  if (props.selected.includes(img)) {
    emit('update:selected', props.selected.filter((i) => i !== img))
  } else {
    emit('update:selected', [...props.selected, img])
  }
}
</script>

<style scoped lang="scss">
.carousel {
  position: relative;

  .viewport {
    overflow: hidden;
  }

  .track {
    display: flex;
    transition: transform 0.4s ease;
  }

  .item {
    flex-shrink: 0;
    padding: 4px;
  }

  .image {
    width: 100%;
    height: 140px;
    object-fit: cover;
    cursor: pointer;
    border-radius: 10px;
    transition: transform 0.3s ease, box-shadow 0.3s ease;

    &:hover {
      transform: scale(1.05);
    }

    &.selected {
      box-shadow: 0 0 0 4px #3b82f6;
      transform: scale(0.95);
    }
  }

  .nav {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    border-radius: 50%;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
    z-index: 1;

    &.left {
      left: 0;
    }

    &.right {
      right: 0;
    }
  }
}

@media (max-width: 768px) {
  .carousel {
    .item {
      width: 100% !important;
    }
  }
}
</style>
