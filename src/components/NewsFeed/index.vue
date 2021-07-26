<template>
  <div id="news-feed" >
    <h1 class="text-center text-2xl py-4">newsFeed</h1>
    <!-- <div v-for="story in this.articles" :key="story"> -->
    <div v-for="(story, ind) in this.articles" :key="ind" class="shadow-lg rounded-lg bg-white p-6 mb-6">
      <img class="pr-2 pb-2 inline-block w-1/5" :src=story.thumbnail_standard />
      <div class="inline-block w-4/5">
        <p>{{formatDate(story.published_date)}}</p>
        <p><a :href="story.url" target="_blank">{{story.title}}</a></p>
      </div>
      <p class>{{story.abstract}}</p>
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
    },
    formatDate(timestamp) {
      let day = timestamp.split('T')[0];
      let mm = day.split('-')[1];
      let dd = day.split('-')[2];
      let yyyy = day.split('-')[0];

      return `${mm}-${dd}-${yyyy}`
    },
  },
  created: function() {
    this.loadArticles();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#news-feed {
  margin: 0 auto;
  width: 80%;
}
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
