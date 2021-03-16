<template>
  <article class="text-base my-10">
    <RichTextRenderer
      :document="richTextData"
      :node-renderers="renderNodes()"
      :mark-renderers="renderMarks()"
    />
  </article>
</template>

<script>
import { BLOCKS, MARKS, INLINES } from '@contentful/rich-text-types'
import RichTextRenderer from 'contentful-rich-text-vue-renderer'

export default {
  components: {
    RichTextRenderer,
  },
  props: {
    richTextData: {
      type: Object,
      default() {
        return { message: 'hello' }
      },
    },
  },
  methods: {
    renderMarks() {
      return {
        [MARKS.BOLD]: (text, key, h) =>
          h('strong', { key, attrs: { class: 'font-bold' } }, text),
        [MARKS.ITALIC]: (text, key, h) =>
          h('em', { key, attrs: { class: 'italic text-blue-700' } }, text),
        [MARKS.UNDERLINE]: (text, key, h) =>
          h('span', { key, attrs: { class: 'underline' } }, text),
        [MARKS.CODE]: (text, key, h) =>
          h(
            'code',
            {
              key,
              attrs: { class: 'block bg-grey-100 font-mono text-grey-700' },
            },
            text
          ),
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
        [BLOCKS.HEADING_1]: (node, key, h, next) =>
          h(
            'h1',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.HEADING_2]: (node, key, h, next) =>
          h(
            'h2',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.HEADING_3]: (node, key, h, next) =>
          h(
            'h3',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.HEADING_4]: (node, key, h, next) =>
          h(
            'h4',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.HEADING_5]: (node, key, h, next) =>
          h(
            'h5',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.HEADING_6]: (node, key, h, next) =>
          h(
            'h6',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.UL_LIST]: (node, key, h, next) =>
          h(
            'ul',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.OL_LIST]: (node, key, h, next) =>
          h(
            'ol',
            { key, attrs: { class: '' } },
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
        // Example from https://github.com/contentful/rich-text/issues/88#issuecomment-484084643
        // Trying to return only li not li>p
        //

        [BLOCKS.QUOTE]: (node, key, h, next) =>
          h(
            'blockquote',
            {
              key,
              attrs: {
                class:
                  'my-6 ml-4 pl-4 text-gray-700 border-gray-700 border-l-4 italic font-serif',
              },
            },
            next(node.content, key, h, next)
          ),
        [BLOCKS.HR]: (node, key, h, next) =>
          h(
            'hr',
            { key, attrs: { class: 'border-gray-700' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.EMBEDDED_ENTRY]: (node, key, h, next) =>
          h(
            '',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [BLOCKS.EMBEDDED_ASSET]: (node, key, h, next) =>
          h(
            '',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [INLINES.EMBEDDED_ENTRY]: (node, key, h, next) =>
          h(
            '',
            { key, attrs: { class: '' } },
            next(node.content, key, h, next)
          ),
        [INLINES.HYPERLINK]: (node, key, h, next) =>
          h(
            'a',
            { key, attrs: { class: 'text-primary hover:underline' } },
            next(node.content, key, h, next)
          ),
        [INLINES.ENTRY_HYPERLINK]: (node, key, h, next) =>
          h(
            'nuxt-link',
            { key, attrs: { class: 'text-primary hover:underline' } },
            next(node.content, key, h, next)
          ),
        [INLINES.ASSET_HYPERLINK]: (node, key, h, next) =>
          h(
            '',
            { key, attrs: { class: 'text-green-200' } },
            next(node.content, key, h, next)
          ),
      }
    },
  },
}
</script>

<style></style>
