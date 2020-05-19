<template>
  <div class="uk-section">
    <div class="uk-container uk-container-small uk-position-relative">
      <div class="uk-position-relative uk-margin-medium">

    <!-- Post section -->
    <article class="uk-article">
      <h1 class="uk-article-title"><a class="uk-link-reset" href="">{{post.title}}</a></h1>
      <p class="uk-article-meta" v-if="post.user">Written by {{ post.user.username }} on {{moment(post.created_at).format("LL")}}.</p>
      <hr class="uk-margin-medium">
      <p class="uk-article-meta">Likes: <p>{{ post.like_count}}</p><span uk-icon="icon: heart" class="uk-padding-small uk-padding-remove-left"></span></p>
      <p v-if="post.content" id="editor" v-html="$md.render(post.content)"></p>
      <div class="uk-grid-small uk-child-width-auto" uk-grid>
          <div>
              <a class="uk-button uk-button-text" href="#" v-if="post.comments">{{post.comments.length}} Comments</a>
          </div>
      </div>
      <hr class="uk-margin-medium">
    </article>

    <!-- Comment section -->
    <div class="uk-section-muted uk-margin-medium">
      <div class="uk-container uk-margin-small">
        <div v-for="comment in post.comments">
          <br>
          <header class="uk-comment-header">
            <ul class="uk-comment-meta uk-subnav">
              <li> {{ comment.like_count }} </li>
              <li> {{ moment(comment.created_at).format("LLL") }} </li>
            </ul>
          </header>
          <div class="uk-comment-body uk-padding">{{ comment.content }}</div>
          <br>
        </div>
      </div>
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
