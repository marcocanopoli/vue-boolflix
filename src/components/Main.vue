<template>
    <main>
        <div v-if="searchArray.length > 0" class="cards">
            <ul>
                <Card   
                    v-for="item in searchArray" 
                    :key="item.id"
                    :cover="getPoster(item)"
                    :title="getTitle(item)"
                    :original-title="getOriginalTitle(item)"
                    :lang="item.original_language"
                    :stars="ratingStars(item.vote_average)"
                    :overview="item.overview"/>
            </ul>

        </div>
        <h1 v-else>No results for current search</h1>
    </main>
</template>

<script>
import Card from './Card.vue';

export default {
    name: 'Main',
    // data() {
    //     return {

    //     }
    // },
    components: {
        Card
    },
    props: {
        searchArray: Array
    },
    methods: {
        getTitle(obj) {
            if ('title'in obj) {
                return obj.title;
            } else {
                return obj.name;
            } 
        },
        getOriginalTitle(obj) {
            if ('original_title'in obj) {
                return obj.original_title;
            } else {
                return obj.original_name;
            } 
        },
        getPoster(obj) {
            if (obj.poster_path != null) {
                return 'https://image.tmdb.org/t/p/w342' + obj.poster_path;
            }
        },
        ratingStars(rating) {
            return Math.ceil(parseInt(rating) / 2);
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/mixins.scss';
    
    main {
        height: 100%;

        h1 {
            @include absolute-center;
        }

        .cards {
            margin: 0 auto;
        }

        ul {            
            display: flex;
            flex-wrap: wrap;
            list-style: none;
        }
    }
</style>