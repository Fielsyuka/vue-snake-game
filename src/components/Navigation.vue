<template>
  <div class="navigation">
    <div class="navigation-item">
      <span class="navigation-label">Length: </span>
      <div class="navigation-item-number-container">
        <div class="num-board">{{ length }}</div>
      </div>
    </div>
    <div class="navigation-item">
      <span class="navigation-label">Difficulty: </span>
      <div class="navigation-item-number-container">
        <span class="num-board">{{ difficulty }}</span>
        <div class="difficulty-button-container">
          <div class="difficulty-button difficulty-up" :class="{ 'is-hidden': buttonUpHidden }" @click="onUpDifficulty"></div>
          <div class="difficulty-button difficulty-down" :class="{ 'is-hidden': buttonDownHidden }" @click="onDownDifficulty"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Difficulty } from "../constants";
export default {
  name: "Navigation",
  props: {
    updateDifficulty: Function,
    length: Number,
    difficulty: Number,
  },
  data() {
    return {
      buttonUpHidden: false,
      buttonDownHidden: false,
    };
  },
  methods: {
    onUpDifficulty() {
      this.updateDifficulty(this.difficulty + 1);
    },
    onDownDifficulty() {
      this.updateDifficulty(this.difficulty - 1);
    },
  },
  watch: {
    difficulty(newVal) {
      this.buttonUpHidden = newVal >= Difficulty.length ? true : false;
      this.buttonDownHidden = newVal <= 1 ? true : false;
    },
  },
};
</script>
