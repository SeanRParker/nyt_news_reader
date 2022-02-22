<template>
  <div id="news-feed" class="sm:w-full md:w-4/5 my-0 mx-auto">
    <div class="header flex py-6">
      <select class="flex-1" @change="pickLimit($event)" v-model="currentLimit">
        <option v-for="(num, ind) in limits" :value="num" :key="ind" >{{num}}</option>
      </select>
      <h1 class="flex-1 text-center text-2xl select-none">newsFeed</h1>
      <select class="flex-1"  @change="pickSection($event)" v-model="currentSection">
        <option v-for="(num, ind) in allSections" :value="num.section" :key="ind">{{num.display_name}}</option>
      </select>
    </div>
    <div class="grid grid-flow-row sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      <div v-for="(story, ind) in this.articles" :key="ind" class=" shadow-lg rounded-lg bg-white gap-1.5 p-6 mb-4 align-top">
        <div class="flex card-heading">
          <img v-if="story.thumbnail_standard" class="flex-none pb-4 pt-1.5 align-top max-h-24" :src=story.thumbnail_standard />
          <img v-else class="flex-none pb-4 pt-1.5 align-top max-h-24" src="../../assets/article-icon.png" />
          <div class="flex-1 pl-2">
            <p class="text-lg"><a :href="story.url" target="_blank">{{story.title}}</a></p>
            <p class="text-sm py-2">{{formatDate(story.published_date)}}</p>
          </div>
        </div>
        <p v-if="story.abstract">{{story.abstract}}</p>
        <p v-else><b>Summary not available, click above to see full story</b></p>
      </div>
    </div>
    <div id="footer" class="sticky bottom-0 bg-white">
      <button v-for="(section, i) in sections" :key="i" @click="loadArticles(section, currentLimit)" class="inline-block w-1/4 py-4">{{section}}</button>
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
      currentSection: 'all',
      sections: ['all', 'world', 'business', 'arts'],
      limits: [],
      currentLimit: 50,
      // https://api.nytimes.com/svc/news/v3/content/all/all.json?limit=500&api-key=[YOUR_API_KEY]
      allSsections: [],
    };
  },
  methods: {
    loadArticles(section, limit) {
      fetch(this.BASE_URL + `all/${section}.json?$limit=${limit}&api-key=` + this.apiKey)
      .then(response => response.json())
      .then(data => {
        this.articles = data.results;
      })
      .catch(err => console.log(err));
      scroll(0,0);
      this.currentSection = section;
    },
    loadSections() {
      fetch(this.BASE_URL + `section-list.json?api-key=` + this.apiKey)
      .then(response => response.json())
      .then(data => {
        this.allSections = data.results;
        this.allSections.unshift({section: "all", display_name: "All"});
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
    createLimitAmounts() {
      for(let i = 50; i <= 500; i += 50) {
        this.limits.push(i)
      }
    },
    pickLimit(event) {
      this.loadArticles(this.currentSection, event.target.value);
    },
    pickSection(event) {
      this.loadArticles(event.target.value, this.currentLimit);
      this.currentSection = event.target.value;
    }
  },
  created: function() {
    this.createLimitAmounts();
    this.loadSections();
    this.loadArticles('all', this.currentLimit);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a {
  color: #42b983;
}
</style>
