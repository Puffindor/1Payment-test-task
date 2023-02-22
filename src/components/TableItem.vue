<template>
  <div class="table">
    <span>{{ id }}</span>
    <span>{{ brand }}</span>
    <span>{{ age }}</span>
  </div>
</template>

<script>
export default {
  props: ["id", "age", "brand"],
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.table {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
</style>
