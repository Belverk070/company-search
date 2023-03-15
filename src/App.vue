<template>
  <CompanySearch />
</template>

<script>
import CompanySearch from './components/CompanySearch.vue';

export default {
  name: 'App',
  components: {
    CompanySearch,
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
