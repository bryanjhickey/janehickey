<template>
  <div class="container">
    <h1>Inner West Church</h1>
    <ul>
      <li
        v-for="article in articles.articleCollection.items"
        :key="article.articleID"
      >
        {{ article.title }}
      </li>
    </ul>
  </div>
</template>

<script>
import { gql } from 'nuxt-graphql-request'
export default {
  async asyncData({ $graphql, params }) {
    const query = gql`
      query {
        articleCollection(order: sys_publishedAt_ASC) {
          items {
            title
          }
        }
      }
    `
    const articles = await $graphql.default.request(query)
    return { articles }
  },
}
</script>

<style></style>
