<template>
  <div
    class="rating"
    ref="rating"
    :class="{ fixed: isSelecting }"
    :style="ratingStyle"
    :data-value="hoveredRating || currentRating"
    @mousemove="handleMouseMove"
    @mouseleave="resetRating"
    @click="fixRating"
  >
    <div class="rating-empty" :style="emptyIconStyle"></div>
    <div class="rating-fill" :style="fillIconStyle"></div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from "vue";

import StarIcon from "../assets/icon/star-fill.svg";
import StarEmptyIcon from "../assets/icon/star.svg";

const RATING_COUNT = 5;

const props = defineProps({
  modelValue: {
    type: Number,
    default: 0,
  },
  width: {
    type: Number,
    default: 150,
  },
  fillIcon: {
    type: String,
    default: StarIcon,
  },
  emptyIcon: {
    type: String,
    default: StarEmptyIcon,
  },
});
const emit = defineEmits(["update:modelValue"]);

const rating = ref<HTMLElement | null>(null);
const hoveredRating = ref(0);
const currentRating = ref(0);
const isSelecting = ref(false);

watch(
  () => props.modelValue,
  (value) => {
    currentRating.value = value;
  }
);

const ratingStyle = computed(() => {
  const width = props.width || 150;
  const height = width / RATING_COUNT;
  const backgroundSize = `${height}px ${height}px`;

  return {
    width: `${width}px`,
    height: `${height}px`,
    backgroundSize,
  };
});

const emptyIconStyle = computed(() => {
  return {
    backgroundImage: `url(${props.emptyIcon})`,
    ...ratingStyle.value,
  };
});

const fillIconStyle = computed(() => {
  const ratingValue = hoveredRating.value || currentRating.value;
  const percentage = 100 - (ratingValue / RATING_COUNT) * 100;
  const clipPathValue = `inset(0 ${percentage}% 0 0)`;

  return {
    backgroundImage: `url(${props.fillIcon})`,
    clipPath: clipPathValue,
    ...ratingStyle.value,
  };
});

const handleMouseMove = (e: MouseEvent) => {
  if (!rating.value) return;

  const rect = rating.value.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const width = rect.width;

  let value = Math.ceil((x / width) * 10) / 2;
  value = Math.min(Math.max(value, 0.5), RATING_COUNT);

  hoveredRating.value = value;
};

const fixRating = () => {
  if (hoveredRating.value === 0) return;

  currentRating.value = hoveredRating.value;
  emit("update:modelValue", currentRating.value);

  isSelecting.value = true;
  setTimeout(() => {
    isSelecting.value = false;
  }, 200);
};

const resetRating = () => {
  hoveredRating.value = 0;
};
</script>

<style scoped>
.rating {
  position: relative;
  transition: 0.2s;
  transform: scale(1);
  cursor: pointer;
}

.rating-empty,
.rating-fill {
  position: absolute;
  top: 0;
  left: 0;
  background-repeat: repeat-x;
}

.rating.fixed {
  transform: scale(1.02);
}
</style>
