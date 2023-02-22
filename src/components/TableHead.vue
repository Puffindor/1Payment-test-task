<template>
  <div class="container">
    <div class="input-container">
      <span style="margin-right: 5px">Search by</span>
      <input
        class="search"
        placeholder="ID or Brand"
        v-model="this.searchInput"
        @input="Search"
      />
      <div>
        <span> Year from </span>
        <input
          min="0"
          max="2023"
          type="number"
          v-model="this.yearMin"
          @input="Search"
        />
        <span> to </span>
        <input
          min="0"
          max="2023"
          type="number"
          v-model="this.yearMax"
          @input="Search"
        />
      </div>
    </div>

    <div class="table-head">
      <span class="field" @click="sort('id')">ID</span>
      <span class="field" @click="sort('brand')">Brand</span>
      <span class="field" @click="sort('age')">Age</span>
    </div>
    <div>
      <div class="f" v-for="item in items" :key="item.id">
        <TableItem :id="item.id" :age="item.age" :brand="item.brand" />
      </div>
    </div>
  </div>
</template>

<script>
import TableItem from "./TableItem.vue";
export default {
  components: { TableItem },
  props: ["items"],
  data() {
    return {
      counter: 0,
      searchInput: "",
      yearMin: "",
      yearMax: "",
    };
  },
  methods: {
    Search() {
      this.$emit("search", this.searchInput, this.yearMin, this.yearMax);
    },
    sort(field) {
      this.counter += 1;
      switch (this.counter) {
        case 1:
          this.$emit("sort", field, true);
          break;
        case 2:
          this.$emit("sort", field, false);
          break;
      }
      if (this.counter >= 2) {
        this.counter = 0;
      }
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.search {
  margin-bottom: 10px;
  outline: none;
}
input {
  outline: none;
}

.field {
  cursor: pointer;
  user-select: none;
}
.input-container {
  border: 1px solid black;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}
.container {
  flex-direction: column;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
}
.table-head {
  font-weight: 700;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
  background-color: rgb(184, 182, 182);
  padding: 5px;
  width: 80vw;
  border: 1px solid black;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
.f:last-child {
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
}

.f {
  padding-top: 5px;
  padding-bottom: 5px;
  width: 80vw;
  border: 1px solid black;
  border-top-style: hidden;
}
</style>
