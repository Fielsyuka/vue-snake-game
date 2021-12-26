<template>
  <div class="field">
    <template v-for="row in fields">
      <div v-for="(col, index) in row" :class="['dots', col]" :key="index"></div>
    </template>
  </div>
</template>
<script>
export default {
  name: "Field",
  props: {
    fields: Array,
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
.dots {
  height: 9px;
  width: 9px;
  background: #0f1d1d;
}

.field {
  display: flex;
  flex-wrap: wrap;
}

.snake {
  background: white;
}

.food {
  background: pink;
}
</style>
