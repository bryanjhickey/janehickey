<template>
  <div>
    <global-alert />
    <global-header />
    <Nuxt />
    <global-footer />
  </div>
</template>
<script>
import GlobalHeader from '~/components/global/GlobalHeader.vue'
import GlobalFooter from '~/components/global/GlobalFooter.vue'
import GlobalAlert from '~/components/global/GlobalAlert.vue'

export default {
  components: {
    GlobalHeader,
    GlobalFooter,
    GlobalAlert,
  },
}
</script>

<style></style>
