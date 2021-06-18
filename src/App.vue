<template>
  <div id="app">
    <Header @search="performSearch"/>
    <Loader v-if="loading"/>
    <Main  v-else
          :all-results="allResults"
          :movies="movies"
          :series="series"
          :movie-genres="movieGenres"
          :tv-genres="tvGenres"
          :my-api-key="myApiKey"
          :current-search="currentSearch"/>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';
import Loader from './components/Loader.vue';
import axios from 'axios';

//font
import "@fontsource/arimo";

export default {
  name: 'App',
  data() {
    return {
      myApiKey: 'cac0ead6a46196b7a9b38d946de02afc',
      apiUrl: 'https://api.themoviedb.org/3/search/',
      movies: [],
      series: [],
      allResults: [],
      movieGenres: [],
      tvGenres: [],
      currentSearch: '',
      loading: false
    }
  },
  components: {
    Header,
    Main,
    Loader
  },
  methods: {
    performSearch(text) {
      this.loading = true;
      this.currentSearch = text;
      
      if (text.trim() == '') {
        this.movieSeries = [];        
        this.movies = [];        
        this.series = [];
        this.loading = false;               
      } else if (text.trim() != '') {

        axios.all([

          //movies
          axios.get(this.apiUrl + 'movie', {
            params: {
              api_key: this.myApiKey,
              query: text
            }      
          }),

          //series
          axios.get(this.apiUrl + 'tv', {
            params: {
              api_key: this.myApiKey,
              query: text
            }      
          })
        ])
        .then(axios.spread((...res) => {
          const movieRes = res[0].data.results;
          const tvRes = res[1].data.results;
          this.movies = movieRes;
          this.series = tvRes;
          this.movieSeries = movieRes.concat(tvRes);
          this.loading = false;        
        }));
      }
    },
    getMovieGenres() {           
      axios
        .get('https://api.themoviedb.org/3/genre/movie/list', {
            params: {
                api_key: this.myApiKey                        
            }
        })
        .then(res => {
            this.movieGenres = (res.data.genres);
        })
        .catch(error => {
            console.log(error);
        });
    }, 
    getTvGenres() {           
      axios
        .get('https://api.themoviedb.org/3/genre/tv/list', {
            params: {
                api_key: this.myApiKey                        
            }
        })
        .then(res => {
            this.tvGenres = (res.data.genres);
        })
        .catch(error => {
            console.log(error);
        });
    } 
  },
  created() {
    this.getMovieGenres();
    this.getTvGenres();
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
