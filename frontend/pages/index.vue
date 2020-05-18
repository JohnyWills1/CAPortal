<template>
  <div class="uk-container uk-container-large">
    <h1 class="uk-heading-large">CAPortal</h1>
    <Threads :threads="threads"></Threads>
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
