<template>
  <div
    class="rating"
    ref="rating"
    :style="ratingStyle"
    @mousemove="handleMouseMove"
    @mouseleave="resetRating"
    @click="fixRating"
  >
    <div
      v-for="(value, index) in RATING_COUNT"
      :key="index"
      class="star"
      :style="iconContainerStyle"
      :class="{ filled: isFilled(value) }"
    >
      <div class="star-empty" :style="iconStyle(emptyIcon)"></div>
      <div class="star-fill" :style="iconStyle(fillIcon, value)"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from "vue";
import StarIcon from "/icon/star-fill.svg";
import StarEmptyIcon from "/icon/star.svg";

const RATING_COUNT = 5;

const props = defineProps({
  modelValue: {
    type: Number,
    default: 0,
  },
  gap: {
    type: Number,
    default: 2,
  },
  iconSize: {
    type: Number,
    default: 30,
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
const currentRating = ref(props.modelValue);

watch(
  () => props.modelValue,
  (value) => {
    currentRating.value = value;
  }
);

const ratingValue = computed(() => hoveredRating.value || currentRating.value);

const ratingStyle = computed(() => ({
  display: "flex",
  justifyContent: "center",
  gap: `${props.gap}px`,
}));

const iconContainerStyle = computed(() => ({
  width: `${props.iconSize}px`,
  height: `${props.iconSize}px`,
}));

const iconStyle = (icon: string, value?: number) => {
  const percentage = value ? getClipPathPercent(value) : 100;
  const clipPathValue = `inset(0 ${percentage}% 0 0)`;

  return {
    width: `${props.iconSize}px`,
    height: `${props.iconSize}px`,
    backgroundImage: `url(${icon})`,
    backgroundSize: "contain",
    backgroundRepeat: "no-repeat",
    clipPath: value ? clipPathValue : undefined,
  };
};

const isFilled = (v: number) => getClipPathPercent(v) < 100;

const getClipPathPercent = (v: number) => {
  if (v <= ratingValue.value) return 0;
  return v - ratingValue.value === 0.5 ? 50 : 100;
};

const handleMouseMove = (e: MouseEvent) => {
  if (!rating.value) return;

  const rect = rating.value.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const width = rect.width;

  const value = Math.ceil((x / width) * RATING_COUNT * 2) / 2;
  hoveredRating.value = Math.min(Math.max(value, 0.5), RATING_COUNT);
};

const fixRating = () => {
  if (hoveredRating.value === 0) return;

  currentRating.value = hoveredRating.value;
  emit("update:modelValue", currentRating.value);
};

const resetRating = () => {
  hoveredRating.value = 0;
};
</script>

<style scoped>
.rating {
  width: fit-content;
  display: flex;
  cursor: pointer;
}

.star {
  position: relative;
  transition: transform 0.2s ease;
}

.star:hover,
.star.filled {
  transform: scale(1.2);
}

.star-empty,
.star-fill {
  position: absolute;
  top: 0;
  left: 0;
  transition: clip-path 0.2s ease;
}
</style>
