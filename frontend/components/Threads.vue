  <template>
  <div>

    <!-- Admin Dashboard -->
    <div v-if="$auth.loggedIn">
      <div v-if="$auth.user.role.name === 'Admin' ">
        Admin Dashboard.
      </div>
    </div>

    <div class="uk-container uk-container-large uk-position-relative">
      <div class="uk-position-relative uk-margin-medium">
        <div class="uk-position-top-right uk-margin-bottom">
          <ul class="uk-iconnav">
            <li><button class="uk-button uk-button-secondary uk-link-reset" v-on:click="topicSuggest()">Suggest a Topic!</button></li>
          </ul>
        </div>
      </div>

      <hr class="uk-margin-medium">

      <div class="uk-padding">
        <div class="uk-grid-collapse uk-child-width-1-4@s" uk-grid>
          <nuxt-link v-for="thread in threads"  :to="{ name: 'thread-id', params: { id: thread.id }}" class="uk-link-reset" :key="thread.id">
            <div v-if="thread.Image">
              <div v-if="thread.admin_accepted" class="uk-height-medium uk-flex uk-flex-center uk-flex-middle uk-background-cover uk-light" :alt="thread.name" :data-src="api_url + thread.Image.url" uk-img>
                <h1 class="uk-text-center uk-text-emphasis">{{thread.name}}</h1>
              </div>
            </div>
          </nuxt-link>
        </div>
      </div>

    </div>
  </div>
</template>

  <!-- After some painstaking trail and error I finally figured out uploading as form data to upload:

              var formData = {
                  files: [fs.createReadStream(image_path)],
                  path: 'folder_to_upload'
              }
              request.post({url:`${strapiUrl}/upload`, formData: formData}, ()=>{});

  I could NOT get it working with axios, but going straight to source with using 'request' made it through. Sending in as formData allowed request to set the content types correctly.
  I did this in node, but I'd imagine it would work in front-end react code as well -->

<script>
import threadsQuery from '~/apollo/queries/thread/threads';

export default {
  data() {
    return {
      api_url: process.env.strapiBaseUri,
      threads: [],
      thread_name: 'test',
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
    }
  }
</script>
