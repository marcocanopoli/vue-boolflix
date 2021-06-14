<template>
  <main>
      <ul>
          <li v-for="item in searchArray" :key="item.id">
              <h2>Title: {{ item.title }}</h2>
              <h3>Original title: {{ item.original_title }}</h3>
              <h4>Original language: {{ item.original_language }}</h4>
              <h4>Rating: {{ item.vote_average }}</h4>
            </li>
      </ul>
  </main>
</template>

<script>
export default {
    name: 'Main',
    data() {
        return {

        }
    },
    props: {
        searchArray: Array
    }
}
</script>

<style lang="scss" scoped>

</style>