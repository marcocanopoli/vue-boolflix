<template>
    <li class="card">
        <img class="poster" :src="cover" alt="">
        <div class="card-info">
            <h2>{{ title }}</h2>
            <h3>Original title: 
                <span class="brand-color-l-plus15">{{ originalTitle }}</span>
            </h3>
            <h4>Language: 
                <img 
                    class="flag" 
                    :src="getFlag(lang)" 
                    :alt="lang">
            </h4>
            <div class="stars">
                <span>
                    <i  v-for="index in stars" 
                        :key="index" 
                        class="fas fa-star"></i>
                </span>
                <span>
                    <i  v-for="index in (5 - stars)" 
                        :key="index" 
                        class="far fa-star"></i>
                </span>
            </div>
            <p>{{ overview }}</p>
        </div>        
    </li>
</template>

<script>
export default {
    name: 'Card',
    data() {
        return {
            itFlagPath: require('../assets/img/flags/it.png'),
            enFlagPath: require('../assets/img/flags/en.png')
        }
    },
    props: {
        cover: String,
        title: String,
        originalTitle: String,
        lang: String,
        stars: Number,
        overview: String,
    },
    methods: {
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
                    console.log("Unknown flag code for", countryISOCode);
            }
            return path;
        }
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
        margin: 5px;;
        border-radius: 15px;
        box-shadow: 0 0 10px scale-color($bg-color, $lightness: +20%);
        overflow: hidden;
        background: $bg-color url('../assets/img/logo-512.png') no-repeat center center;
        background-size: 50%;

        &:hover {
            cursor: pointer;

            .card-info {
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
                scale-color($bg-color, $alpha: -50%) 20%, 
                scale-color($bg-color, $alpha: -25%) 40%, 
                scale-color($bg-color, $alpha: -2%) 70%);
            opacity: 0;
            transition: .3s;

            h2, h3, h4, p {
                padding-bottom: 5px;
                text-shadow: 0 0 5px $bg-color;                
            }

            h2 {
                padding-bottom: 10px;
                color: $brand-color-l-plus15;
            }
            
            .flag {
                width: 20px;
                margin-left: 5px;
                vertical-align: text-bottom;
            }  
            
            .stars {
                margin-top: 5px;
                i {
                margin-right: 5px;

                    &.fas {
                        color: $brand-color-l-plus15;
                    }
                }
            }

            p {
                margin-top: 10px;
                padding-top: 10px;
                overflow-y: auto;
                border-top: 1px solid $brand-color-l-plus15;
            }
        }               
    }
</style>