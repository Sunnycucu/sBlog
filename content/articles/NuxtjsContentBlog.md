---
title: HTML, CSS, Javascript
description: Learning how to use @nuxt/content to create a blog
img: first-blog-post.jpg
alt: my first blog post
author:
  name: Benjamin
  bio: All about Benjamin

---

# nuxt/content module
writes in content/ directory and fetch Markdown, JSON, YAML, XML,
and CSV files through a MongoDB like API, acting as a Git-based Headless CMS.

- 시작하기전에...
  - npm install @nuxt/content
  
- content/articles directory 안에 md file들을 넣어준다

### content display
- we can use dynamic page by prefixing the page with an underscore('_')
- example : created _slug.vue
- use the params.slug variable provided by vue router to get the name of each article
- have access to our contenct through the context by using the variable $content  
- use asycnData in our page component to fetch our article content before the page has been rendered.
- need to know wich article to fetch -> params.slug(avaiable through the context)
- fetched our content using the await folled by $content. (pass into content what we want to fetch, which is the article folder)

```
<script>
  export default {
    async asyncData({ $content, params }) {
      const article = await $content('articles', params.slug).fetch()

      return { article }
    }
  }
</script>
```

- use <nuxt-content /> component to display our content. 
- pass in the variable we returned into the document prop.
- run the dev serer and we can go to the route  http://localhost:3000/blog/my-first-blog-post
```
<template>
  <article>
    <nuxt-content :document="article" />
  </article>
</template>
```
### Default injected variable 
- The nuxt content module gives us access to injected variables that we can access and show in our template.
  - body: body text
  - dir : directory
  - extension : file extension(.md in this example)
  - path : the file path
  - slug : the file slug
  - toc : an array containing our table of contents
  - createdAt : the file creation date
  - updatedAt : the date of the last file update
- We can access all these variables by using the article variable that we created earlier.
- example : inspect article object:
```
<pre> {{ article }} </pre>
```
##### if we want to write nicely format the updated date for an article
```
methods: {
formatDate(date) {
    const options = { year: 'numeric', month: 'long', day: 'numeric' }
    return new Date(date).toLocaleDateString('en', options)
    }
}
```

```
<p>Article last updated: {{ formatDate(article.updatedAt) }}</p>
```
### Custom injected variables
- add a block of YAML front matter to the markdown file
- It must be at the top of the file and must be a valid YAML format
- example : 

```
---
title: My first Blog Post
description: Learning how to use @nuxt/content to create a blog
img: first-blog-post.jpg
alt: my first blog post
---
```
- Then, we can access by using our article object variable.

```
<template>
  <article>
    <h1>{{ article.title }}</h1>
    <p>{{ article.description }}</p>
    <img :src="article.img" :alt="article.alt" />
    <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

    <nuxt-content :document="article" />
  </article>
</template>
```

### Styling the markdown content
- can easily add styles to all our elements coming from our markdown file by wrapping them in the nuxt-content class.
```
<style>
  .nuxt-content h2 {
    font-weight: bold;
    font-size: 28px;
  }
  .nuxt-content h3 {
    font-weight: bold;
    font-size: 22px;
  }
  .nuxt-content p {
    margin-bottom: 20px;
  }
</style>
```
### Adding an icon to our headings anchor
- irst add the svg to your assets folder
```
.icon.icon-link {
  background-image: url('~assets/svg/icon-hashtag.svg');
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
```
### Add a table of contents
- if we add headings to our blog post(e.g ## this is heading), we can see these inside the toc array with an id, a depth and the text.
- As we have access to the toc id and text we can loop over these and print each one out and use the <NuxtLink> component to link to the id of the section we want to link to.
```
<nav>
  <ul>
    <li v-for="link of article.toc" :key="link.id">
      <NuxtLink :to="`#${link.id}`">{{ link.text }}</NuxtLink>
    </li>
  </ul>
</nav>
```

### use HTML in the markdown files
 - we can add this to our markdown files. 
```
<div class="p-4 mb-4 text-white bg-blue-500">
  This is HTML inside markdown that has a class of note
</div>
```
### Adding a Vue component
- if we are re-using components, we can create one with the styles we need and pass in the text as a slot.
- in nuxt.config.js
```
export default {
components: true
}
```
- auto importing compoenents will not work for <nuxt-content> unless we globally register them by adding a global folder inside the components folder.
```
mkdir components/global
```
- for an example, we can create our InfoBox compoenent(InfoBox.vue)
```
<template>
  <div class="p-4 mb-4 text-white bg-blue-500">
    <p><slot name="info-box">default</slot></p>
  </div>
</template>
```
- then in our markdown, we can use them :
```
  <info-box>
  <template #info-box>
  This is a vue component inside markdown using slots
  </template>
  </info-box>
```

### Adding an Author compoenent with props


### Creating a previous and next component

### List all the blog posts
- 
