<template>
    <main>
        <div v-if="currentSearch == ''" class="welcome">
            <h1>Welcome to</h1>
            <img src="../assets/img/notflix-logo.png" alt="">
        </div>

        <!-- Movies section -->
        <Section 
            :title="'Movies'"
            :results="movies"
            :search="currentSearch"
            :filtered="filteredMovies"
            :genres="movieGenres"
            :my-api-key="myApiKey"
            :media="'movie'"
            @sendFilter="saveMovieFilter"/>
        <!-- /Movies section -->

        <!-- TV section -->
        <Section 
            :title="'Series'"
            :results="series"
            :search="currentSearch"
            :filtered="filteredSeries"
            :genres="tvGenres"
            :my-api-key="myApiKey"
            :media="'tv'"
            @sendFilter="saveTvFilter"/>
        <!-- /TV section -->
    </main>
</template>

<script>
import Section from './Section.vue';

export default {
    name: 'Main',
    components: {
        Section,
    },
    data() {
        return {
            movieFilter: '',
            tvFilter: '',
            movieGenreId: '',
            tvGenreId: ''
        }
    },
    props: {
        // allResults: Array,
        movies: Array,
        series: Array,
        movieGenres: Array,
        tvGenres: Array,
        myApiKey: String,
        currentSearch: String
    },
    methods: {
        saveMovieFilter(filter, genreId){
            this.movieFilter = filter;
            this.movieGenreId = genreId;
        },        
        saveTvFilter(filter, genreId){
            this.tvFilter = filter;
            this.tvGenreId = genreId;
        }
    }
    ,
    computed: {
        
        filteredMovies() {
            let filtered = [];
            if (this.movieGenreId == '' ) {
                filtered = this.movies; 
            } else {
                filtered = this.movies.filter(movie => movie.genre_ids.includes(this.movieGenreId));
            }
            return filtered;
        },
        filteredSeries() {
            let filtered = [];
            if (this.tvGenreId == '' ) {
                filtered = this.series; 
            } else {
                filtered = this.series.filter(serie => serie.genre_ids.includes(this.tvGenreId));
            }
            return filtered;
        }     
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    @include pulse-animation;
    
    main {
        min-height: calc(100vh - #{$header-height});                        
        background-image: linear-gradient((rgba($bg-color, 0.7)), (rgba($bg-color, 0.4)));

        .welcome {
            @include absolute-center;  
            text-align: center;
            filter: drop-shadow(0 0 100px rgba($brand-color, 0.7));
            animation: pulse 5s alternate infinite;
            
            h1 {
                font-size: 100px;
            }

            img {
                width: 515px;
            }

            h1, img {
                filter: drop-shadow(0 0 10px $bg-color);
            }
        }
    }

    
</style>