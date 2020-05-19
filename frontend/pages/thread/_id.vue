<template>
  <div class="uk-section">
    <div class="uk-container uk-container-small uk-position-relative">
      <div class="uk-position-relative uk-margin-medium">
        <div class="uk-position-top-right uk-margin-small-top">
          <ul class="uk-iconnav">
            <li><a href="#" uk-icon-="icon: plus"></a></li>
            <li><a href="#" uk-icon="icon: file-edit"></a></li>
          </ul>
        </div>
      </div>
      <hr class="uk-margin-medium">
      <router-link v-for="post in thread.posts" :to="{ name: 'posts-id', params: { id: post.id }}" :key="post.id">
        <ul class="uk-list uk-list-striped">
          <li v-if="post.title">
            {{ post.title }}
          </li>
        </ul>
      </router-link>
    </div>
  </div>
</template>

<script>
import thread_postsQuery from '~/apollo/queries/thread/thread_posts'

var moment = require('moment')

export default {
  data() {
    return {
      thread: {},
      moment: moment,
      api_url: process.env.strapiBaseUri
    }
  },
  apollo: {
    thread: {
      prefetch: true,
      query: thread_postsQuery,
      variables() {
        return {
          id: parseInt(this.$route.params.id)
        }
      }
    }
  }
}
</script>
