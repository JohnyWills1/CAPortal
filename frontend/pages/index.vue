<template>
  <div>
    <div class="uk-container uk-container-large uk-position-relative">
      <div class="uk-container">
        <h1 class="uk-heading-large">Home</h1>
      </div>
      <div uk-alert>
          <a class="uk-alert-close" uk-close></a>
          <p>This site adheres to GDPR laws and will securely store you information. Some cookies will be stored on your local machine.</p>
      </div>
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
