<template>
  <div class="uk-section">
    <div class="uk-container uk-container-large uk-position-relative">
      <h1 class="uk-heading-large">Home</h1>
      <Threads :threads="threads"></Threads>
    </div>
  </div>
</template>
<nuxt/>

<script>
import threadsQuery from '~/apollo/queries/thread/threads'
import Threads from '~/components/Threads'

export default {
  data() {
    return {
      threads: [],
    }
  },
  components: {
    Threads
  },
  apollo: {
    threads: {
      prefetch: true,
      query: threadsQuery
    }
  }
}
</script>
