<script setup lang="ts">
import { ref } from "vue";
import FaceEmptyIcon from "./assets/icon/face.svg";
import FaceFillIcon from "./assets/icon/face-fill.svg";

import Rating from "./components/Rating.vue";

const rating = ref(0);
</script>

<template>
  <header class="header">
    <h1>Vue Simple Rating Component</h1>
    <a href="https://github.com/KumJungMin/v-simple-rating" key="github-link">
      GitHub Link
    </a>
  </header>
  <div class="content">
    <h2>Rating</h2>
    <h4>Default Rating</h4>
    <div class="code-container">
      <code>
        <span class="keyword">&lt;Rating /&gt;</span>
      </code>
    </div>

    <Rating />
    <br />
    <hr />
    <br />

    <h4>Custom Rating</h4>
    <Rating
      :fillIcon="FaceFillIcon"
      :icon-size="50"
      :gap="10"
      :emptyIcon="FaceEmptyIcon"
      v-model="rating"
    />
    <div>
      <p>rating: {{ rating }}</p>
      <button @click="rating = 3">change rating to 3</button>
    </div>
    <br />

    <div class="code-container">
      <code>
        <span class="keyword">&lt;Rating</span>
        :fillIcon=<span class="value">"FaceFillIcon"</span> :icon-size=<span
          class="value"
          >"50"</span
        >
        :gap=<span class="value">"10"</span> :emptyIcon=<span class="value"
          >"FaceEmptyIcon"</span
        >
        v-model=<span class="value">"rating"</span>
        <span class="keyword">/&gt;</span>
      </code>
    </div>
  </div>
</template>

<style>
.header {
  text-align: center;
  backdrop-filter: blur(10px);
  padding: 20px;
  background-color: #e0e0e0;
}

.code-container {
  background-color: #2e2e2e;
  color: #e0e0e0;
  border-radius: 5px;
  padding: 15px;
  overflow-x: auto;
  max-width: 100%;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
pre {
  margin: 0;
  font-size: 16px;
  line-height: 1.5;
}
code {
  display: block;
  text-align: left;
}
.keyword {
  color: #f92672;
  font-weight: bold;
}
.attribute {
  color: #66d9ef;
}
.value {
  color: #a6e22e;
}
</style>
