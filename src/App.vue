<template>
  <div id="app">
    <Header @sendSearch="performSearch"/>
    <Main :searched="searched"/>
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
      apiMovieUrl: 'https://api.themoviedb.org/3/search/movie',
      apiTvUrl: 'https://api.themoviedb.org/3/search/tv',
      searched: []
    }
  },
  components: {
    Header,
    Main
  },
  methods: {
    performSearch(text) {
      if (text.trim() == '') {
        this.searched = [];        
      } else if (text.trim() != '') {

        axios.all([

          //movies
          axios.get(this.apiMovieUrl, {
            params: {
              api_key: this.myApiKey,
              query: text
            }      
          }),

          //series
          axios.get(this.apiTvUrl, {
            params: {
              api_key: this.myApiKey,
              query: text
            }      
          })
        ])
        .then(axios.spread((...res) => {
          const movieRes = res[0].data.results;
          const tvRes = res[1].data.results;
          this.searched = movieRes.concat(tvRes);
        }));
      }
    } 
  }
}
</script>

<style lang="scss">
  @import './assets/style/global.scss';

  #app {
    min-height: 100vh;
    background-image: url('./assets/img/bg2.jpg');
    background-repeat: repeat-y;
  }
</style>
