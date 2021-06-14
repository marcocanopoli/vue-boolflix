<template>
  <header>
        <img src="../assets/img/notflix-logo.png" alt="Not!flix Logo">
        <nav>
            <input               
                type="text" 
                placeholder="Movies, series, titles..."
                v-model="searchInput"
                @keyup="$emit('sendSearch', searchInput)">
                <!-- search on enter or button -->
                <!-- @keydown.enter="$emit('sendSearch', searchInput)" -->
            <!-- <button @click="$emit('sendSearch', searchInput)">Search</button> -->
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
        height: 65px;
        padding: 10px 0;

        img {
            height: 60%;
        }

        nav {
            width: 100%;
            display: flex;
            justify-content: flex-end;  
            
            input,
            button {
                padding: 5px;
                margin-left: 10px;
                font-size: 18px;
                border-radius: 10px;
                color: $text-color;
                background-color: transparent;
            }
        }
    }
</style>