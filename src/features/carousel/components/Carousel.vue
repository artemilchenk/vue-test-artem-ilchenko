<template>
  <div class="carousel">
    <CarouselNavButton
      class="nav-button prev"
      @click="prev"
    >
      <slot name="prev-button">
        prev
      </slot>
    </CarouselNavButton>

    <div
      ref="carouselRef"
      class="viewport"
    >
      <div
        class="track"
        :style="{ transform: `translateX(-${index * itemWidth}%)` }"
      >
        <div
          v-for="(img, i) in loopedImages"
          :key="i"
          class="item"
          :style="{ width: `${itemWidth}%` }"
        >
          <div @click="onItemClick(img)">
            <slot
              name="item"
              :img="img"
              :selected="selected.includes(img)"
            >
              <CarouselItem
                :img="img"
                :is-selected="selected.includes(img)"
              />
            </slot>
          </div>
        </div>
      </div>
    </div>

    <CarouselNavButton
      class="nav-button next"
      @click="next"
    >
      <slot name="next-button">
        next
      </slot>
    </CarouselNavButton>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import CarouselNavButton from "../ui/CarouselNavButton.vue";
import CarouselItem from "./CarouselItem.vue";

const emit = defineEmits(["selectChange", "next", "prev"]);

const props = defineProps({
  images: {
    type: Array,
    default: () => [],
  },
  itemsOnView: { type: Number, default: 4 },
});

const index = ref(0);
const selected = ref([]);
const itemWidth = computed(() => 100 / props.itemsOnView);

const loopedImages = computed(() => [...props.images, ...props.images]);

function prev() {
  index.value = index.value === 0 ? props.images.length - 1 : index.value - 1;

  emit("prev", {
    index: index.value,
  });
}

function next() {
  index.value = (index.value + 1) % props.images.length;

  emit("next", {
    index: index.value,
  });
}

function toggleSelectItem(img) {
  const isSelected = selected.value.includes(img);

  let nextSelected;

  if (isSelected) {
    nextSelected = selected.value.filter((i) => i !== img);
  } else {
    nextSelected = [...selected.value, img];
  }

  selected.value = nextSelected;

  emit("selectChange", {
    item: img,
    selected: nextSelected,
    isSelected: !isSelected,
  });
}

function onItemClick(img) {
  toggleSelectItem(img);
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
    box-sizing: border-box;
    flex-shrink: 0;
    padding: 4px;
  }

  .nav-button {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    z-index: 1;

    &.prev {
      left: 8px;
    }

    &.next {
      right: 8px;
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
