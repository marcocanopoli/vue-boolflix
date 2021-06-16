<template>
    <li class="card">
        <img class="poster" :src="getPoster(item)" alt="">
        <div class="card-info">            
            <h2 class="title">{{ getTitle(item) }}</h2>
            <h3 class="original">Original title: 
                <span>{{ getOriginalTitle(item) }}</span>
            </h3>
            <h4 class="lang">Language: 
                <img 
                    class="flag" 
                    :src="getFlag(item.original_language)" 
                    :alt="item.original_language">
            </h4>
            <div class="stars">
                <i  v-for="i in 5" 
                    :key="i" 
                    :class="i <= getStars(item.vote_average) ? 'fas' : 'far'"
                    class="fa-star"></i>
            </div>
            <div class="genres" v-if="genresStrings.length > 0">
                <span   class="genre" 
                        v-for="genre, index in genresStrings" 
                        :key="index">
                        {{ genre }}
                </span>
            </div>
            <div class="cast" v-if="cast.length > 0">
                <span   v-for="actor in cast" 
                        :key="actor.id">
                        {{ actor.name }} : <span>{{actor.character}}</span>
                </span>
            </div>
            <p class="overview">{{ item.overview }}</p>
        </div>        
    </li>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Card',
    data() {
        return {
            itFlagPath: require('../assets/img/flags/it.png'),
            enFlagPath: require('../assets/img/flags/en.png'),
            creditsUrl: `https://api.themoviedb.org/3/${this.media}/${this.item.id}/credits`,
            genresListUrl: `https://api.themoviedb.org/3/genre/${this.media}/list`,
            cast: [],
            genres: [],
            genresStrings: [],
        }
    },
    props: {
        media: String,
        item: Object,
        apiKey: String
    },
    methods: {
        //get item's first 5 cast names
        getCast() { 
            axios
                .get(this.creditsUrl, {
                    params: {
                        api_key: this.apiKey                        
                    }
                })
                .then(res => {
                    this.cast = (res.data.cast);
                    this.cast.splice(5);
                })
                .catch(error => {
                    console.log(error);
                });
        },

        //get all genres for item's media (movie or tv)
        getGenres() {           
            axios
                .get(this.genresListUrl, {
                    params: {
                        api_key: this.apiKey                        
                    }
                })
                .then(res => {
                    this.genres = (res.data.genres);
                    this.getGenresStrings();
                    console.log(this.genresStrings);
                })
                .catch(error => {
                    console.log(error);
                });
        },

        //get current item's genres
        getGenresStrings() {
            this.item.genre_ids.forEach(genreId => {
                this.genres.forEach(genre => {
                    if (genre.id == genreId) {
                        this.genresStrings.push(genre.name);
                    }
                });                
            });
            return this.genresStrings;
        },

        //get current item's poster
        getPoster(obj) {
            if (obj.poster_path != null) {
                return 'https://image.tmdb.org/t/p/w342' + obj.poster_path;
            }
        },

        //get current item's title
        getTitle(obj) {
            if (obj.title) {
                return obj.title;
            } else {
                return obj.name;
            } 
        },

        //get current item's original title
        getOriginalTitle(obj) {
            if (obj.original_title) {
                return obj.original_title;
            } else {
                return obj.original_name;
            } 
        },

        //get current item's language flag
        getFlag (countryISOCode) {
            let path = '';
            switch(countryISOCode) {

                case 'it':
                    path = this.itFlagPath;
                    break;
                case 'en':
                    path = this.enFlagPath;
                    break;
                default:
                    // console.log("Unknown flag code for", countryISOCode);
            }
            return path;
        },

        //get current item's 1-to-5 stars rating
        getStars(rating) {
            return Math.ceil(parseInt(rating) / 2);
        },
    },
    created() {
        this.getCast();
        this.getGenres();
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    .card {
        position: relative;     
        height: 507px;
        min-height: 507px;
        width: 342px;
        min-width: 342px;
        margin: 9px;
        border-radius: 15px;
        border: 3px solid $bg-color;
        box-shadow: 0 0 10px rgba($bg-color, 0.5);
        overflow: hidden;
        background: $bg-color url('../assets/img/logo-512.png') no-repeat center center;
        background-size: 50%;

        &:hover {
            cursor: pointer;
            border: 5px solid $brand-color;
            transition-delay: .3s;
            transition-duration: .5s;
            transform: scale(1.2);
            z-index: 1;

            .card-info {
                transition-delay: .5s;
                opacity: 1;
            }
        }

        .poster {
            min-height: 100%;
            object-fit: cover;            
        }

        .card-info {
            @include absolute-center;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            height: 100%;
            max-height: 100%;
            width: 100%;
            padding: 15px;
            text-align: center;
            background-image: linear-gradient(
                transparent, 
                scale-color($bg-color, $alpha: -50%) 15%, 
                scale-color($bg-color, $alpha: -15%) 40%, 
                scale-color($bg-color, $alpha: -2%) 70%);
            opacity: 0;
            transition: .3s;

            & > * {
                position: relative;                
                text-shadow: 0 0 5px $bg-color;
                padding-bottom: 5px;
            }

            .title,
            .original > span,
            .stars > .fas ,
            .cast > span > span{
                color: $brand-color-l-plus15;
            }            
            
            .flag {
                width: 20px;
                margin-left: 5px;
                vertical-align: text-bottom;
            } 
            
            .stars {
                i {
                    margin-right: 5px;
                }
            }   

            .genres {
                padding-bottom: 10px;
                .genre:not(:last-child)::after {
                    content: ',';
                }
            }

            .cast {
                display: flex;
                flex-direction: column;
                padding: 10px 0;
                
                span {
                    overflow-x: hidden;
                    white-space: nowrap;
                    text-overflow: ellipsis;
                }                
            } 

            .genres,
            .cast {
                &::after {
                    content: '';
                    position: absolute;
                    bottom: 0;
                    left: 50%;
                    transform: translateX(-50%);                   
                    width: 35%;                    
                    border-bottom: 1px solid $brand-color-l-plus15;
                }        
            }

            .overview {
                margin-top: 10px;
                font-size: 14px;
                overflow-y: auto;
            }
        }               
    }
</style>