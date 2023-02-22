<template>
  <div>
    <Table :items="result" @search="search" @sort="sort" />
  </div>
</template>

<script>
import Table from "./components/TableHead.vue";
export default {
  components: { Table },
  data() {
    return {
      items: [
        { id: 1, brand: "BMW", age: 2010 },
        { id: 4, brand: "Audi", age: 2018 },
        { id: 3, brand: "BMW", age: 2016 },
        { id: 2, brand: "Audi", age: 2012 },
      ],
      result: [],
    };
  },
  mounted() {
    this.result = this.items;
  },
  methods: {
    //////////////////////////////////////////////////////
    // Не совсем понял пункт с запросом на бэк поэтому, как пример, просто передаю поле
    //по которому в данный момент идет сортировка, с "-" если от большего к меньшему и без него если наоборот.
    //В консоли появляется соответствующие ошибки в виду отсутствия самого бэка.
    //////////////////////////////////////////////////////
    async request(field, reverse) {
      const init = {
        method: "PUT",
      };
      if (reverse > 0) {
        await fetch(`https:localhost:3005/cars/index?sort=${field}`, init);
      } else {
        await fetch(`https:localhost:3005/cars/index?sort=-${field}`, init);
      }
    },
    //////////////////////////////////////////////////////
    sort(field, reverse) {
      this.request(field, reverse);
      function dynamicSort(property, reverse) {
        let sortOrder = reverse ? 1 : -1;
        return function (a, b) {
          let result =
            a[property] < b[property] ? -1 : a[property] > b[property] ? 1 : 0;
          return result * sortOrder;
        };
      }

      this.items.sort(dynamicSort(field, reverse));
    },

    search(e, min, max) {
      if (e) {
        this.result = this.items.filter(
          (el) =>
            el.brand.toLocaleLowerCase().includes(e.toLocaleLowerCase()) ||
            e == el.id
        );
      } else this.result = this.items;
      if (min && max) {
        this.result = this.result.filter((h) => h.age >= min && h.age <= max);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
