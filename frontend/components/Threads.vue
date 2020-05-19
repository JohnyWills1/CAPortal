  <template>
  <div>

    <div class="uk-container uk-container-large uk-position-relative">
      <div class="uk-position-relative uk-margin-medium">
        <div class="uk-position-top-right uk-margin-small-top">
          <ul class="uk-iconnav">
            <li><a href="#" uk-icon-="icon: plus"></a></li>
            <li><a href="#" uk-icon="icon: file-edit"></a></li>
          </ul>
        </div>
      </div>
      <hr class="uk-margin-medium">

      <div class="uk-grid-collapse uk-child-width-1-4@s" uk-grid>
        <router-link v-for="thread in threads" :to="{ name: 'thread-id', params: { id: thread.id }}" class="uk-link-reset" :key="thread.id">
          <div class="uk-height-medium uk-flex uk-flex-center uk-flex-middle uk-background-cover uk-light" :alt="thread.name" :data-src="api_url + thread.Image[0].url" uk-img>
            <h1 class="uk-text-center uk-text-emphasis">{{thread.name}}</h1>
          </div>
        </router-link>
      </div>

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
