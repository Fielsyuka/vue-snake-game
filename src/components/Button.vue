<template>
  <div class="button">
    <button class="btn btn-init" @click="$emit('onChangeStatus', getStatusVal('playing'))" v-if="status === getStatusVal('init')">
      Start
    </button>
    <button class="btn btn-suspended" @click="$emit('onChangeStatus', getStatusVal('playing'))" v-if="status === getStatusVal('suspended')">
      Start
    </button>
    <button class="btn btn-playing" @click="$emit('onChangeStatus', getStatusVal('suspended'))" v-if="status === getStatusVal('playing')">
      Stop
    </button>
    <button class="btn btn-gameover" @click="$emit('onChangeStatus', getStatusVal('init'))" v-if="status === getStatusVal('gameover')">
      Game Over
    </button>
  </div>
</template>

<script>
import { GameStatus } from "../constants";

export default {
  name: "Button",
  props: {
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
