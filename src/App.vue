<template>
  <CurrencyConverter />
  <CurrencyTable />
</template>

<script>
import CurrencyConverter from './components/CurrencyConverter.vue';
import CurrencyTable from './components/CurrencyTable.vue';

export default {
  name: 'App',
  components: {
    CurrencyConverter,
    CurrencyTable
  }
}
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
