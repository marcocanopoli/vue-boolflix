<template>
    <main>
        <div v-if="movies.length > 0">
            <h2>Movies</h2>
            <ul>
                <Card   
                    v-for="item in movies" 
                    :key="item.id"
                    :item="item"
                    :media="'movie'"
                    :api-key="myApiKey"/>
            </ul>
        </div>
        <div v-if="series.length > 0">
            <h2>TV</h2>
            <ul>
                <Card   
                    v-for="item in series" 
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
    props: {
        movieSeries: Array,
        movies: Array,
        series: Array,
        myApiKey: String
    // },
    // methods: {
    //     getCat(obj) {
    //         if (obj.title) {
    //             return 'movie';
    //         } else if (obj.name) {
    //             return 'tv';
    //         }            
    //     }
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

        h2 {
            width: 342px;
            margin: 0 auto;
            text-align: center;
            font-size: 48px;
            background: linear-gradient(90deg, transparent 0%, $brand-color 50%, transparent 100%);
        }
    }
</style>