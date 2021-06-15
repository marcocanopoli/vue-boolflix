<template>
    <main>
        <ul>
            <Card   
                v-for="item in searched" 
                :key="item.id"
                :cover="getPoster(item)"
                :title="getTitle(item)"
                :original-title="getOriginalTitle(item)"
                :lang="item.original_language"
                :stars="ratingStars(item.vote_average)"
                :overview="item.overview"/>                
        </ul>               
    </main>
</template>

<script>
import Card from './Card.vue';

export default {
    name: 'Main',
    components: {
        Card
    },
    props: {
        searched: Array
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
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';
    
    main {
        min-height: calc(100vh - #{$header-height});                        
        background-image: linear-gradient((rgba($bg-color, 0.7)), transparent);

        ul {                       
            display: flex;
            flex-wrap: wrap;
            padding: 40px 50px;
            list-style: none;
            // background-color: rgba($bg-color, 0.7);
        }
    }
</style>