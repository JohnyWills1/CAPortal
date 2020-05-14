  <template>
  <div>
    <div class="uk-text-center uk-grid-match uk-child-width-expand@s" uk-grid>
      <router-link v-for="thread in threads" :to="{ name: 'threads-id', params: { id: thread.id }}" class="uk-link-reset" :key="thread.id">
        <div class="uk-card uk-card-default">
          <div class="uk-card-media-top">
            <img :src="api_url + thread.Image[0].url" :alt="thread.name" width="400" height="400">
          </div>
          <div class="uk-card-body">
            <div class="uk-card-badge uk-label">{{ thread.name }}</div>
          </div>
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
