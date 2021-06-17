<template>
  <div class="loader">
        <Spinner />
        <h1>Please wait while content is loading...</h1>
  </div>
</template>

<script>
import Spinner from './Spinner.vue';

export default {
    name: 'Loader',
    components: {
        Spinner
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/mixins.scss';
    @import '../assets/style/variables.scss';

    .loader {
        @include flex-center('both');
        flex-direction: column;
        min-height: calc(100vh - #{$header-height});  
        width: 100%;                       
        background-image: linear-gradient((rgba($bg-color, 0.7)), (rgba($bg-color, 0.4))); 
        text-align: center;
        filter: drop-shadow(0 0 10px $bg-color);
        
        & > h1,
        & > div {
            transform: translateY(-$header-height);
        }
        h1 {
            font-size: 72px;
        }
    }

</style>