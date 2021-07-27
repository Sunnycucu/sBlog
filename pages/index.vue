<template>
  <div>


    <div class="mainExplain">
      <img  class="actLogo" src="/act.png" />
      <div class="websiteName">
       Global Internship
        <p class="name">Andrew & Sunny</p>
    </div>
    </div>

    <div class="cardContainer">
      <div class="card" v-for="article of articles" :key="article.slug">
        <div class="aImageContainer">
          <img class="articleImage" :src="article.img" />
        </div>
        <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
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
  margin: 70px;
  width : 40%;
  padding : 15px;
  flex-direction: column;



}

.cardContainer{
  margin: 50px;
  display: flex;
  justify-content: center;
  border-top: solid black 1px;
  flex-flow : row wrap;
}

.mainExplain {
  height : 300px;
  margin: 20px 20px 40px;
  display: flex;
  align-content: center;
}

.websiteName {
  margin: 20px 20px 20px 40px;
  display: flex;
  font-weight: bold;
  flex-direction: column;
  justify-content: center;
  font-family: "Roboto Mono";
  font-size: 50px;
}
.actLogo {
  width: 280px;
  height: 280px;
}
.name {
  padding-left: 20px;
  font-size: 25px;
}
.articleImage {
  width: 650px;
  height: 350px;
}
</style>
