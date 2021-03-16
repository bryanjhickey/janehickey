<template>
  <div class="container max-w-lg lg:max-w-3xl mx-auto p-4 pt-32 text-pink-900">
    <div
      v-for="biography in biographies.biographyCollection.items"
      :key="biography.biographyID"
    >
      <img
        class="rounded-full h-32 w-32 md:h-48 md:w-48 my-6"
        :src="`${biography.bioImage.url}?f=face&fit=thumb&h=800&w=800`"
        :alt="biography.bioImage.title"
      />
      <h1 class="text-2xl md:text-4xl uppercase mb-2">
        {{ biography.name }}
      </h1>
      <h2 class="text-base font-bold tracking-normal mb-2 text-orange-900">
        {{ biography.currentRole }}
      </h2>
      <p class="text-xs italic text-gray-700">
        {{ biography.academicQualifications }}
      </p>
      <contentful-rich-text :rich-text-data="biography.description.json" />
    </div>
  </div>
</template>

<script>
import { gql } from 'nuxt-graphql-request'
import ContentfulRichText from '~/components/partials/typography/ContentfulRichText.vue'

export default {
  components: {
    ContentfulRichText,
  },
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
    // eslint-disable-next-line no-console
    console.log(biographies)
    return { biographies }
  },
}
</script>

<style></style>
