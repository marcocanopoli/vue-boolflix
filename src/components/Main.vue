<template>
    <main>
        <div v-if="movies.length > 0">
            <h2>Movies</h2>
            <!-- <h4>Results for "{{ movieFilter }}"</h4>            -->
            <div class="select-box">
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
            <h3 v-if="filteredMovies.length == 0">No results for current filter</h3>
            <ul>
                <Card   
                    v-for="item in filteredMovies" 
                    :key="item.id"
                    :item="item"
                    :media="'movie'"
                    :api-key="myApiKey"/>
            </ul>
        </div>
        <div v-if="series.length > 0">
            <h2>TV</h2>
            <!-- <h4>Results for "{{ tvFilter }}"</h4>              -->
            <div class="select-box">
                <select name="movie-genres" id="movie-genres"
                        v-model="tvFilter"
                        @change="getTvGenreId(tvFilter)">
                    <option value=""
                            @click="tvGenreId  = ''">All series</option>
                    <option v-for="genre in tvGenres" 
                            :key="genre.id"
                            :value="genre.name">{{ genre.name }}
                    </option>
                </select>                
            </div>
            <h3 v-if="filteredSeries.length == 0">No results for current filter</h3>
            <ul>
                <Card   
                    v-for="item in filteredSeries" 
                    :key="item.id"
                    :item="item"
                    :media="'tv'"
                    :api-key="myApiKey"/>
            </ul>
        </div>
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
        myApiKey: String
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
        },
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

        h2 {
            width: 342px;
            margin: 0 auto;
            text-align: center;
            font-size: 48px;
            background: linear-gradient(90deg, transparent 0%, $brand-color 50%, transparent 100%);
        }

        h3 {
            text-align: center;
            font-size: 32px;
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