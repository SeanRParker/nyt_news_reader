<template>
  <div id="news-feed">
    <h1>newsFeed</h1>
    <!-- <div v-for="story in this.articles" :key="story"> -->
    <div v-for="story in this.articles" :key="story.slug_name">
      <h3>{{story.title}}</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NewsFeed',
  data: () => {
    return {
      BASE_URL: "https://api.nytimes.com/svc/news/v3/content/",
      apiKey: "42E7VHsQarerg6mapRfsNc4hwcWmP3QH",
      articles: []
    };
  },
  methods: {
    loadArticles() {
      fetch(this.BASE_URL + `all/all.json?api-key=` + this.apiKey)
      .then(response => response.json())
      .then(data => {
        this.articles = data.results;
        console.log(this.articles);
      })
      .catch(err => console.log(err));
    }
  },
  created: function() {
    this.loadArticles();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
