<template>
  <div id="app">
    <HomePage />
  </div>
</template>

<script>
import HomePage from './pages/index.vue';

export default {
  name: 'App',
  components: {
    HomePage,
  },
};
</script>

<style lang="scss">
@import './assets/styles/_base.scss';
</style>
