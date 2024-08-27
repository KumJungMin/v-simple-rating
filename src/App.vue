<script setup lang="ts">
import { ref } from "vue";
import FaceEmptyIcon from "./assets/icon/face.svg";
import FaceFillIcon from "./assets/icon/face-fill.svg";

import Rating from "./components/Rating.vue";

const rating = ref(0);
</script>

<template>
  <Rating />

  <hr />
  <Rating
    :fillIcon="FaceFillIcon"
    :icon-size="50"
    :gap="10"
    :emptyIcon="FaceEmptyIcon"
    v-model="rating"
  />
  <p>rating: {{ rating }}</p>
  <button @click="rating = 3">change rating to 3</button>
</template>
