<template>
  <div>

    <div class="uk-section">
      <div class="uk-container uk-container-large">
        <h1>CA-Portal</h1>

        <Threads :threads="threads"></Threads>

      </div>
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
      query: threadsQuery,
      variables () {
        return { id: parseInt(this.$route.params.id) }
      }
    }
  }
}
</script>
