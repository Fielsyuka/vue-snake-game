<template>
  <div class="button">
    <button class="btn btn-init" @click="onStart" v-if="status === getStatusVal('init')">Start</button>
    <button class="btn btn-suspended" @click="onStart" v-if="status === getStatusVal('suspended')">Start</button>
    <button class="btn btn-playing" @click="onStop" v-if="status === getStatusVal('playing')">Stop</button>
    <button class="btn btn-gameover" @click="onRestart" v-if="status === getStatusVal('gameover')">Game Over</button>
  </div>
</template>

<script>
import { GameStatus } from "../constants";

export default {
  name: "Button",
  props: {
    onStart: Function,
    onRestart: Function,
    status: String,
  },
  data() {
    return {};
  },
  methods: {
    getStatusVal(key) {
      return GameStatus[key];
    },
  },
};
</script>
