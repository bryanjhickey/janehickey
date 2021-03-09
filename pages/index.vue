<template>
  <div class="container mx-auto pt-32 font-mono">
    <div
      v-for="biography in biographies.biographyCollection.items"
      :key="biography.biographyID"
    >
      <img
        class="rounded-full h-64 w-64"
        :src="`${biography.bioImage.url}?f=face&fit=thumb&h=800&w=800`"
        :alt="biography.bioImage.title"
      />
      <h1 class="text-5xl font-hairline">{{ biography.name }}</h1>
      <h2 class="font-bold text-lg">{{ biography.currentRole }}</h2>
      <p class="text-base italic text-gray-700">
        {{ biography.academicQualifications }}
      </p>
      <article class="text-lg my-10">
        <div v-html="biography.description" />
      </article>
    </div>
  </div>
</template>

<script>
import { gql } from 'nuxt-graphql-request'
export default {
  async asyncData({ $graphql, params }) {
    const query = gql`
      query {
        biographyCollection {
          items {
            name
            bioImage {
              title
              url
            }
            currentRole
            academicQualifications
            description {
              json
            }
          }
        }
      }
    `
    const biographies = await $graphql.default.request(query)
    return { biographies }
  },
}
</script>

<style></style>
