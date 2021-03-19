<template>
  <div class="container max-w-lg lg:max-w-3xl mx-auto p-4 pt-32">
    <RichTextRenderer :document="document" :node-renderers="renderNodes()" />
  </div>
</template>

<script>
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
          value: 'Hello',
          marks: [{ type: 'bold' }],
        },
        {
          nodeType: 'text',
          value: ' world!',
          marks: [{ type: 'italic' }],
        },
      ],
    },
    {
      nodeType: 'unordered-list',
      content: [
        {
          nodeType: 'list-item',
          content: [
            {
              nodeType: 'paragraph',
              content: [
                {
                  nodeType: 'text',
                  value: 'First List item ',
                  marks: [{ type: 'bold' }],
                },
              ],
            },
          ],
        },
      ],
    },
  ],
}

export default {
  components: {
    RichTextRenderer,
  },
  data() {
    return {
      document,
    }
  },
  methods: {
    renderMarks() {
      return {
        [MARKS.BOLD]: (text, key, h) => h('strong', { key }, text),
      }
    },
    renderNodes() {
      return {
        [BLOCKS.PARAGRAPH]: (node, key, h, next) =>
          h(
            'p',
            { key, attrs: { class: 'my-4' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.LIST_ITEM]: (node, key, h, next) => {
          node.content = node.content
            .map((listItemNode) =>
              listItemNode.nodeType === BLOCKS.PARAGRAPH
                ? listItemNode.content
                : listItemNode
            )
            .flat()
          return h(
            'li',
            { key, attrs: { class: 'list-disc list-inside ml-8' } },
            next(node.content, key, h, next)
          )
        },
      }
    },
  },
}

console.log(document)
</script>

<style></style>
