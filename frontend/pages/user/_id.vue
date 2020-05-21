<template>
  <div class="uk-section">
    <div class="uk-container uk-container-small uk-position-relative uk-text-center">
      <h1 class="uk-heading-large uk-text-center">{{ $auth.user.username }}</h1>
      <hr class="uk-margin-medium">
      <div v-if="$auth.user.profilePicture">
        <img :src="api_url + $auth.user.profilePicture.url" style="border-radius: 50%;" width="100" height="100">
      </div>
      <button class="uk-button uk-button-secondary">Change Picture</button>
      <hr class="uk-margin-medium">

    </div>
  </div>
</template>

<script>
import thread_postsQuery from '~/apollo/queries/thread/thread_posts'

export default {
  data() {
    return {
      api_url: process.env.strapiBaseUri
    }
  }
}
</script>
