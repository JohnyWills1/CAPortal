<template>

<div>
  <div class="uk-section">
    <div class="uk-container uk-container-small">
      <div v-if="post.content" id="editor" v-html="$md.render(post.content)"></div>
      <p v-if="post.created_at">{{ moment(post.created_at).format("LL") }}</p>
    </div>
  </div>

  <div class="uk-section-muted">
    <div class="uk-container">
      <div v-for="comment in post.comments">
        <br>
        <header class="uk-comment-header">
          <ul class="uk-comment-meta uk-subnav">
            <li> {{ comment.like_count }} </li>
            <li> {{ moment(comment.created_at).format("LLL") }} </li>
          </ul>
        </header>
        <div class="uk-comment-body">{{ comment.content }}</div>
        <br>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import postQuery from '~/apollo/queries/post/post'

var moment = require('moment')

export default {
  data() {
    return {
      post: {},
      moment: moment,
      api_url: process.env.strapiBaseUri
    }
  },
  apollo: {
    post: {
      prefetch: true,
      query: postQuery,
      variables() {
        return {
          id: parseInt(this.$route.params.id)
        }
      }
    }
  }
}
</script>
