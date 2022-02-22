<template>
  <div id="app" class="bg-gray-300 pb-5">
    <NewsFeed />
  </div>
</template>

<script>
import NewsFeed from './components/NewsFeed'

export default {
  name: 'App',
  components: {
    NewsFeed
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
