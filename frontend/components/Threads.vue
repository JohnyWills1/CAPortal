  <template>
  <div>
    <div class="uk-text-center uk-grid-match uk-child-width-expand@s" uk-grid>
      <router-link v-for="thread in threads" :to="{ name: 'threads-id', params: { id: thread.id }}" class="uk-link-reset" :key="thread.id">
        <div class="uk-card uk-card-default uk-card-body uk-width-1-2@m">
            <img :src="api_url + thread.Image[0].url" :alt="thread.name" height="500" width="500">
            <h3 class="uk-card-title">{{ thread.name }}</h3>
       </div>
      </router-link>

    </div>
  </div>
</template>

<script>
import threadsQuery from '~/apollo/queries/thread/threads'

export default {
  data() {
    return {
      api_url: process.env.strapiBaseUri,
      threads: []
    }
  },
  apollo: {
    threads: {
      prefetch: true,
      query: threadsQuery
    }
  }
}
</script>
