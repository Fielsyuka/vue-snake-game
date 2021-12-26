<template>
  <div className="manipulation-panel">
    <button class="manipulation-btn btn btn-left" @click="$emit('onChange', getDirectionVal('left'))">←</button>
    <div>
      <button class="manipulation-btn btn btn-up" @click="$emit('onChange', getDirectionVal('up'))">↑</button>
      <button class="manipulation-btn btn btn-down" @click="$emit('onChange', getDirectionVal('down'))">↓</button>
    </div>
    <button class="manipulation-btn btn btn-right" @click="$emit('onChange', getDirectionVal('right'))">→</button>
  </div>
</template>

<script>
import { Direction } from "../constants";
export default {
  name: "ManipulationPanel",
  methods: {
    getDirectionVal(key) {
      return Direction[key];
    },
  },
};
</script>
