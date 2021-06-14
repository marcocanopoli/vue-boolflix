<template>
  <div id="app">
    <Header />
  </div>
</template>

<script>
import Header from './components/Header.vue';

//font
import "@fontsource/arimo";

export default {
  name: 'App',
  components: {
    Header
  }
}
</script>

<style lang="scss">
  @import './assets/style/global.scss';
  // #app {
    
  // }
</style>
