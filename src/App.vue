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
      myApiKey: 'cac0ead6a46196b7a9b38d946de02afc',
      searchArray: []
    }
  },
  components: {
    Header,
    Main
  },
  methods: {
    performSearch(text) {
      if (text.trim() == '') {
        this.searchArray = [];        
      } else if (text.trim() != '') {

        axios.all([

          //movies
          axios.get('https://api.themoviedb.org/3/search/movie', {
            params: {
              api_key: this.myApiKey,
              query: text
            }      
          }),

          //series
          axios.get('https://api.themoviedb.org/3/search/tv', {
            params: {
              api_key: this.myApiKey,
              query: text
            }      
          })
        ])
        .then(axios.spread((...movieRes) => {
          this.searchArray = movieRes[0].data.results.concat(movieRes[1].data.results);
        }));
      }
    } 
  }
}
</script>

<style lang="scss">
  @import './assets/style/global.scss';

  #app {
    height: 100%;
  }
</style>
