<template>
    <main>
        <div v-if="currentSearch == ''" class="welcome">
            <h1>Welcome to</h1>
            <img src="../assets/img/notflix-logo.png" alt="">
        </div>

        <!-- Movies section -->
        <section v-if="currentSearch != ''">
            <h2>Movies</h2>
            <h4>Results for "{{ currentSearch }}"</h4>            
            <div class="select-box" v-if="movies.length > 0">
                <select name="movie-genres" id="movie-genres"
                        v-model="movieFilter"
                        @change="getMovieGenreId(movieFilter)">
                    <option value=""
                            @click="movieGenreId = ''">All movies</option>
                    <option v-for="genre in movieGenres" 
                            :key="genre.id"
                            :value="genre.name">{{ genre.name }}
                    </option>
                </select>                
            </div>
            <h3 v-if="filteredMovies.length == 0 && movieFilter != ''">
                No results for current filter
            </h3>
            <ul>
                <Card   
                    v-for="item in filteredMovies" 
                    :key="item.id"
                    :item="item"
                    :media="'movie'"
                    :api-key="myApiKey"/>
            </ul>
        </section>
        <!-- /Movies section -->

        <!-- TV section -->
        <section v-if="currentSearch != ''">
            <h2>TV</h2>
            <h4>Results for "{{ currentSearch }}"</h4> 
            <div class="select-box" v-if="series.length > 0">
                <select name="tv-genres" id="tv-genres"
                        v-model="tvFilter"
                        @change="getTvGenreId(tvFilter)">
                    <option value=""
                            @click="tvGenreId = ''">All series</option>
                    <option v-for="genre in tvGenres" 
                            :key="genre.id"
                            :value="genre.name">{{ genre.name }}
                    </option>
                </select>                
            </div>
            <h3 v-if="filteredSeries.length == 0 && tvFilter != ''">
                No results for current filter
            </h3>
            <ul>
                <Card   
                    v-for="item in filteredSeries" 
                    :key="item.id"
                    :item="item"
                    :media="'tv'"
                    :api-key="myApiKey"/>
            </ul>
        </section>
        <!-- /TV section -->

    </main>
</template>

<script>
import Card from './Card.vue';

export default {
    name: 'Main',
    components: {
        Card
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
        movieSeries: Array,
        movies: Array,
        series: Array,
        movieGenres: Array,
        tvGenres: Array,
        myApiKey: String,
        currentSearch: String
    },
    methods: {        
        getMovieGenreId(filter) {
            this.movieGenres.forEach(genre => {                
                if (genre.name == filter) {
                    this.movieGenreId = genre.id;
                }
            });
        },
        getTvGenreId(filter) {
            this.tvGenres.forEach(genre => {                
                if (genre.name == filter) {
                    this.tvGenreId = genre.id;
                }
            });
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
    },
    updated() {

        if (this.currentSearch == '') {
            this.movieFilter = '';
            this.tvFilter = '';
            this.movieGenreId = '';
            this.tvGenreId = '';
        }                 
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';
    
    main {
        min-height: calc(100vh - #{$header-height});                        
        background-image: linear-gradient((rgba($bg-color, 0.7)), (rgba($bg-color, 0.4)));        

        ul {                       
            display: flex;
            flex-wrap: wrap;
            padding: 40px 50px;
            list-style: none;
            // background-color: rgba($bg-color, 0.7);
        }

        .welcome {
            @include absolute-center;  
            text-align: center;
            filter: drop-shadow(0 0 100px rgba($brand-color, 0.7));

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

        h2, h3, h4 {
            text-align: center;
            text-shadow: (0 0 10px $bg-color);
        }

        h2 {
            width: 342px;
            margin: 0 auto;
            font-size: 48px;
            background: linear-gradient(90deg, transparent 0%, $brand-color 50%, transparent 100%);
        }

        h3 {
            font-size: 32px;
        }

        h4 {
            font-size: 24px;
            padding-top: 20px;
        }


        .select-box {
            display: flex;
            justify-content: flex-end;
            padding: 0 50px;

            select {
                padding: 10px 5px;
                border-radius: 10px;
                font-size: 20px;
                color: $text-color;
                background-color: $bg-color;
                outline: none;
                border: 2px solid $brand-color;
            }
        }
    }
</style>