<template>
    <section v-if="search != ''">
        <h2>{{ title }}</h2>
        <h3 v-if="results.length == 0">No results for '{{ search }}'</h3>            
        <h4 v-else>Results for '{{ search }}'</h4>

        <Select 
            :name="title.toLowerCase()"
            :content="results"
            :genres="genres"
            @selectFilter="saveFilter"/>
        <h3 v-if="filtered.length == 0 && filter != ''">
            No results for '{{ filter }}' filter
        </h3>
        <ul>
            <Card   
                v-for="item in filtered" 
                :key="item.id"
                :item="item"
                :media="media"
                :api-key="myApiKey"/>
        </ul>
    </section>
</template>

<script>
import Select from './Select.vue';
import Card from './Card.vue';

export default {
    name: 'Section',
    components: {
        Select,
        Card
    },
    data() {
        return {
            filter: '',
            genreId: ''
        }
    },
    props: {
        title: String,
        results: Array,
        search: String,
        filtered: Array,
        genres: Array,
        myApiKey: String,
        media: String                
    },
    methods: {
        saveFilter (filter, genreId) {
            this.filter = filter;
            this.genreId = genreId;
            this.$emit("sendFilter", filter, genreId);
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    @include pulse-animation;

    section { 
        animation: pulse 1s;       

        h2, h3, h4 {
            text-align: center;
            text-shadow: (0 0 10px $bg-color);
        }

        h2 {
            width: 342px;
            margin: 0 auto 20px;
            font-size: 48px;
            background: linear-gradient(90deg, transparent 0%, $brand-color 50%, transparent 100%);
        }

        h3 {
            font-size: 32px;
        }

        h4 {
            font-size: 24px;
        }
    }

    ul {                       
        display: flex;
        flex-wrap: wrap;
        padding: 40px 50px;
        list-style: none;
        // background-color: rgba($bg-color, 0.7);
    }
</style>