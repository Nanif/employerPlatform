<template>
  <div id="app">
    
    <productsTable></productsTable>
  </div>
</template>

<script>
import productsTable from "./components/productsTable.vue";

export default {
  name: "App",
  components: {
    productsTable
  }
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
