<template>
  <div class="uk-section">
    <div class="uk-container uk-container-small uk-position-relative">
      <div class="uk-position-relative uk-margin-medium uk-text-center">

        <!-- Post section -->
        <article class="uk-article">
          <h1 class="uk-article-title"><a class="uk-link-reset" href="">{{post.title}}</a></h1>
          <p class="uk-article-meta" v-if="post.user">Written by {{ post.user.username }} on {{moment(post.created_at).format("LL")}}.</p>
          <hr class="uk-margin-medium">
          <p class="uk-article-meta uk-text-center">Likes: {{ post.like_count}}<span uk-icon="icon: heart" class="uk-padding-small uk-padding-remove-left"></span></p>
          <p v-if="post.content" id="editor" v-html="$md.render(post.content)"></p>
          <div class="uk-grid-small uk-child-width-auto" uk-grid>
              <div>
                  <a class="uk-button uk-button-text" href="#" v-if="post.comments">{{post.comments.length}} Comments</a>
              </div>
          </div>
          <hr class="uk-margin-medium">
        </article>

        <div class="uk-position-relative uk-margin-medium uk-position-center">
          <div class="uk-margin">
            <ul class="uk-iconnav" v-if="!this.show_comment_box">
              <li><button class="uk-button uk-button-secondary" @click="show_comment_box = !show_comment_box" type="button">Post a Comment!</button></li>
            </ul>
          </div>
        </div>

        <div v-if="this.show_comment_box" class="uk-container uk-position-relative">
          <div class="uk-position-relative uk-margin-medium uk-text-center">
            <form>
              <div v-if="this.show_alert" class="uk-alert-danger" uk-alert>
                <a class="uk-alert-close" uk-close></a>
                <p>You must be logged in to comment.</p>
              </div>
              <fieldset class="uk-fieldset">
                <div class="uk-margin">
                  <textarea class="uk-textarea" rows="5" placeholder="Comment Body" v-model="comment_body"></textarea>
                </div>
                <div class="uk-margin">
                  <button class="uk-button uk-button-secondary" @click="postComment()" type="submit">Post Comment</button>
                </div>
              </fieldset>
            </form>
          </div>
        </div>

        <!-- Comment section -->
        <div class="uk-position-relative uk-margin-medium uk-text-left">
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
                <hr class="uk-margin-small">
                <div class="uk-comment-body uk-padding">{{ comment.content }}</div>
                <br>
              </div>
            </div>
          </div>
        </div>
        
    </div>
  </div>
</div>
</template>

<script>
import postQuery from '~/apollo/queries/post/post';
import axios from 'axios';

var moment = require('moment')

export default {
  data() {
    return {
      post: {},
      show_comment_box: false,
      show_alert: false,
      comment_body: '',
      moment: moment,
      api_url: process.env.strapiBaseUri,

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
  },
  methods: {
    postComment() {
      // if (this.comment_body) {
      //   break;
      // }
      axios
        .post('http://localhost:1337/comments', {
          content: this.comment_body,
          post: this.post,
          user: this.$auth.user
        })
        .then(response => {
          // Handle success.
          console.log('Successfully Posted Comment.');
          this.show_comment_box = false;
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
          this.show_alert = true;
        });
    }
  }
}
</script>
