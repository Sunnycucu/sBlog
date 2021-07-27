<template>
  <div>
    <div class="websiteName">Andrew&Sunny Blog</div>

    <div class="mainExplain">

    </div>

    <div class="cardContainer">
      <div class="card" v-for="article of articles" :key="article.slug">
        <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
          <img :src="article.img" />
          <div>
            <h2>{{ article.title }}</h2>
            <p>by {{ article.author.name }}</p>
            <p>{{ article.description }}</p>
          </div>
        </NuxtLink>
      </div>
    </div>


  </div>
</template>


<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content('articles')
      .only(['title', 'description', 'img', 'slug', 'author'])
      .sortBy('createdAt', 'asc')
      .fetch()

    return {
      articles
    }
  }
}
</script>

<style>


.card {

  display: flex;
  margin: 20px;
  width : 43%;
  padding : 15px;
  background-color: lavender;

}

.cardContainer{
  display: flex;
  flex-flow : row wrap;
}

.mainExplain {
  height : 300px;
  border-bottom: solid black 1px;
  margin: 20px 20px 40px;
}

.websiteName {
  margin: 20px;
  font-family: "Roboto Mono";
  font-size: 35px;
}
</style>
