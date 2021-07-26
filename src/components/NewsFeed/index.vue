<template>
  <div id="news-feed" >
    <h1 class="text-center text-2xl py-4">newsFeed</h1>
    <!-- <div v-for="story in this.articles" :key="story"> -->
    <div v-for="(story, ind) in this.articles" :key="ind" class="shadow-lg rounded-lg bg-white gap-1.5 p-6 mb-4 sm:w-full md:w-1/2 xl:w-1/3 inline-block align-top">
      <img class="pb-4 inline-block w-1/5 pt-1.5 align-top" :src=story.thumbnail_standard />
      <div class="pl-2 inline-block w-4/5">
        <p class="text-lg"><a :href="story.url" target="_blank">{{story.title}}</a></p>
        <p class="text-sm py-2">{{formatDate(story.published_date)}}</p>
      </div>
      <p class>{{story.abstract}}</p>
    </div>
    <div id="footer" class="sticky bottom-0 bg-white">
      <!-- <a v-for="{section, action} in sections" @click="action">{{section}}</a> -->
      <button v-for="(section, i) in sections" :key="i" @click="loadArticles(section)" class="inline-block w-1/4 py-4">{{section}}</button>
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
      articles: [],
      sections: ['all', 'world', 'business', 'arts'],
      // https://api.nytimes.com/svc/news/v3/content/all/all.json?limit=500&api-key=[YOUR_API_KEY]
      // sections: [{
      //   ""
      // }]
    };
  },
  methods: {
    loadArticles(section) {
      fetch(this.BASE_URL + `all/${section}.json?limit=50&api-key=` + this.apiKey)
      .then(response => response.json())
      .then(data => {
        this.articles = data.results;
        console.log(this.articles);
      })
      .catch(err => console.log(err));
      scroll(0,0);
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
    this.loadArticles('all');
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
