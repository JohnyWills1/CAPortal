<template>
  <div class="uk-container">
    <router-link v-for="post in thread.posts" :to="{ name: 'posts-id', params: { id: post.id }}" :key="post.id">
      <ul class="uk-list uk-list-striped">
        <li v-if="post.title">
          {{ post.title }}
        </li>
      </ul>
    </router-link>
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
