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
            <li><button class="uk-button uk-button-primary" uk-toggle="target: #thread_modal" type="button">Suggest a Topic!</button></li>
          </ul>
        </div>
      </div>
      <hr class="uk-margin-medium">

      <div class="uk-grid-collapse uk-child-width-1-4@s" uk-grid>
        <nuxt-link v-for="thread in threads"  :to="{ name: 'thread-id', params: { id: thread.id }}" class="uk-link-reset" :key="thread.id">
          <div v-if="thread.admin_accepted" class="uk-height-medium uk-flex uk-flex-center uk-flex-middle uk-background-cover uk-light" :alt="thread.name" :data-src="api_url + thread.Image[0].url" uk-img>
            <h1 class="uk-text-center uk-text-emphasis">{{thread.name}}</h1>
          </div>
        </nuxt-link>
      </div>

    </div>

    <!-- Create thread modal -->
    <div id="thread_modal" class="uk-modal" uk-modal>
      <div class="uk-modal-dialog uk-modal-body">
        <button class="uk-modal-close-default" type="button" uk-close></button>
        <form @submit.prevent="createThread()">
          <fieldset class="uk-fieldset">

              <h1 class="uk-heading-medium uk-text-center">Suggest a Topic!</h1>
              <hr class="uk-margin-medium">
              <div class="uk-margin uk-text-center">
                  <input class="uk-input uk-form-width-large" type="text" placeholder="Topic Name">
              </div>

              <div class="uk-text-center">
                Topic Image:
              </div>

              <div class="uk-margin uk-text-center">
                  <div uk-form-custom>
                      <input type="file">
                      <button class="uk-button uk-button-default" type="button" tabindex="-1">Upload File</button>
                  </div>
              </div>

              <hr class="uk-margin-medium">

              <div class="uk-margin uk-text-center">
                <button class="uk-button uk-button-primary uk-button-large uk-width-1-1" type="submit">Submit</button>
              </div>

          </fieldset>
        </form>
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
    },
    methods: {
      createThread(){
        const bodyFormData = new FormData();

      }


    }
  }
</script>
