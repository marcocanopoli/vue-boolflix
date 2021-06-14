<template>
  <div id="app">
    <Header @sendSearch="performSearch"/>
    <Main :search-array="searchArray"/>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';
import axios from 'axios';

//font
import "@fontsource/arimo";

export default {
  name: 'App',
  data() {
    return {
      searchArray: []
    }
  },
  components: {
    Header,
    Main
  },
  methods: {
    performSearch(text) {
      if (text.trim() != '') {
        axios
        .get('https://api.themoviedb.org/3/search/movie', {
          params: {
            api_key: 'cac0ead6a46196b7a9b38d946de02afc',
            query: text
          }      
        })
        .then(res => {
          this.searchArray = res.data.results;
        });
      } else if (text.trim() == '') {
        this.searchArray = [];
      }
    } 
  }
}
</script>

<style lang="scss">
  @import './assets/style/global.scss';
  // #app {
    
  // }
</style>
