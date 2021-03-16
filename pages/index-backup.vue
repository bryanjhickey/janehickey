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
      <article>
        <p class="text-base my-10">
          {{ biography.description }}
        </p>
      </article>
      <RichTextRenderer :document="document" :node-renderers="renderNodes()" />
    </div>
  </div>
</template>

<script>
import { gql } from 'nuxt-graphql-request'
import { BLOCKS, MARKS } from '@contentful/rich-text-types'
import RichTextRenderer from 'contentful-rich-text-vue-renderer'

const document = {
  nodeType: 'document',
  content: [
    {
      nodeType: 'paragraph',
      content: [
        {
          nodeType: 'text',
          value: 'bold',
          marks: [{ type: 'bold' }],
        },
        {
          nodeType: 'text',
          value: ' italic!',
          marks: [{ type: 'italic' }],
        },
        {
          nodeType: 'text',
          value: ' underline!',
          marks: [{ type: 'underline' }],
        },
        {
          nodeType: 'hyperlink',
          value: 'hyperlink',
          data: [{ uri: '"https://www.vu.edu.au/"' }],
        },
      ],
    },
    {
      nodeType: 'unordered-list',
      content: [
        {
          nodeType: 'list-item',
          value: 'list item',
        },
      ],
    },
  ],
}

export default {
  components: {
    RichTextRenderer,
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
    return { biographies }
  },
  data() {
    return {
      document,
    }
  },
  methods: {
    renderMarks() {
      return {
        [MARKS.BOLD]: (text, key, h) => h('<strong>', { key }, text),
      }
    },
    renderNodes() {
      return {
        [BLOCKS.PARAGRAPH]: (node, key, h, next) =>
          h(
            '<p class="text-base my-10">',
            { key },
            next(node.content, key, h, next)
          ),
      }
    },
  },
}
</script>

<style></style>
