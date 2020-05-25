  <template>
  <div>
    <div class="uk-container uk-container-large uk-position-relative">
      <div class="uk-position-relative uk-margin-medium">
        <div class="uk-position-top-right uk-margin-bottom" v-if="this.$auth.loggedIn">
          <ul class="uk-iconnav">
            <li><button class="uk-button uk-button-secondary uk-link-reset" v-on:click="topicSuggest()">Suggest a Topic!</button></li>
          </ul>
        </div>
      </div>

      <hr class="uk-margin-medium">

      <div class="uk-padding">
        <div class="uk-grid-collapse uk-child-width-1-5@s" uk-grid>
          <nuxt-link v-for="thread in orderedThreads"  :to="{ name: 'thread-id', params: { id: thread.id }}" class="uk-link-reset" :key="thread.id">
            <div v-if="thread.Image">
              <div v-if="thread.admin_accepted" class="uk-height-medium uk-flex uk-flex-center uk-flex-middle uk-background-cover uk-light" :alt="thread.name" :data-src="api_url + thread.Image.url" uk-img>
                <h3 class="uk-text-center uk-text-emphasis">{{thread.name}}</h3>
              </div>
            </div>
          </nuxt-link>
        </div>
      </div>

    </div>
  </div>
</template>
<script>
import threadsQuery from '~/apollo/queries/thread/threads';

export default {
  data() {
    return {
      api_url: process.env.strapiBaseUri,
      threads: [],
      thread_name: '',
    }
  },
  apollo: {
    threads: {
      prefetch: true,
      query: threadsQuery
      }
    },
    methods: {
      topicSuggest() {
        this.$router.push('/topicSuggest')
      }
    },
    computed: {
      orderedThreads() {
        var _ = require('lodash');
        return _.orderBy(this.threads, ['posts'], ['desc'])
      }
    }
  }
</script>
