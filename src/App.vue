<template>
  <div id="app">
    <SearchBlock />
  </div>
</template>

<script>
import SearchBlock from './components/SearchBlock.vue';

export default {
  name: 'App',
  components: {
    SearchBlock,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
