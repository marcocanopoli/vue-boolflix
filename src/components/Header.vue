<template>
  <header>
        <img src="../assets/img/notflix-logo.png" alt="Not!flix Logo">
        <nav>
            <input               
                type="text" 
                placeholder="Movies, series, titles..."
                v-model="searchInput"
                @keydown.enter="$emit('sendSearch', searchInput)">
                <!-- real-time search -->
                <!-- @keyup="$emit('sendSearch', searchInput)" -->
            <button @click="$emit('sendSearch', searchInput)">Search</button>
      </nav>
  </header>
</template>

<script>
export default {
    name: 'Header',
    data() {
        return {
            searchInput: ''
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    header {
        @include flex-center ('cross');        
        height: $header-height;
        padding: 0 60px; 
        background-image: linear-gradient($bg-color, (rgba($bg-color, 0.7)));

        img {
            height: 50%;
        }

        nav {
            width: 100%;
            display: flex;
            justify-content: flex-end;  
            
            input,
            button {
                padding: 10px;
                margin-left: 10px;
                font-size: 18px;
                border-radius: 10px;
                color: $text-color;
                background-color: transparent;
                outline: none;
                border: 2px solid $brand-color;
            }

            button:hover {
                cursor: pointer;
                color: $brand-color;
            }
        }
    }
</style>