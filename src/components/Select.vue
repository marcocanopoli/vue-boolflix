<template>
    <div class="select-box" v-if="content.length > 0">
        <select :name="`${name}-genres`" :id="`${name}-genres`"
                v-model="filter"
                @change="sendFilter(filter)">
            <option value=""
                    @click="sendFilter('')">All {{name}}</option>
            <option v-for="genre in genres" 
                    :key="genre.id"
                    :value="genre.name">{{ genre.name }}
            </option>
        </select>                
    </div>
</template>

<script>
export default {
    name: 'Select',
    data() {
        return {
            filter: '',
            genreId: '',
        }
    },
    props: {
        name: String,
        genres: Array,
        content: Array
    },
    methods: {
        sendFilter (filter){
            this.getGenreId(filter);
            this.$emit('selectFilter', filter, this.genreId);
        },
        getGenreId(filter) {
            if (filter == '') {
                this.genreId = '';
            } else {
                this.genres.forEach(genre => {                
                    if (genre.name == filter) {
                        this.genreId = genre.id;
                    }
                });
            }
        },
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';

    .select-box {
        position: relative;
        display: flex;
        justify-content: flex-end;
        padding: 0 50px;

        &::after {
            position: absolute;
            content: url('../assets/img/angle-down-solid-white.svg');
            top: 0;
            right: 65px;
            transform: translateY(50%);
            width: 15px;
            pointer-events: none;
        }

        select {
            position: relative;
            padding: 10px 5px;
            border-radius: 10px;
            font-size: 20px;
            color: $text-color;
            background-color: $bg-color;
            outline: none;
            border: 2px solid $brand-color;
            appearance: none;

            &:hover {
                cursor: pointer;
            }
        }
    }         
</style>