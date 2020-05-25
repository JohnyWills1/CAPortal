<template>
  <div class="uk-section">
    <div class="uk-container uk-container-small uk-position-relative">
      <div class="uk-position-relative uk-margin-medium">

        <!-- Post section -->
        <article class="uk-article">
          <div class="uk-text-center">
            <h1 class="uk-article-title"><a class="uk-link-reset" href="">{{post.title}}</a></h1>
            <p class="uk-article-meta" v-if="post.user">Written by {{ post.user.username }} on {{ moment(post.created_at).format("LL") }}.</p>
          </div>
          <hr class="uk-margin-medium">
          <div class="uk-alert-success" v-if="this.show_success" uk-alert>
            <a class="uk-alert-close" uk-close></a>
            <p>Liked Post!</p>
          </div>
          <p class="uk-article-meta uk-text-center" v-if="post.like_count">Likes: {{ post.like_count }} <a @click="likePost()"  uk-icon="icon: heart; ratio: 2" class="uk-padding"></a></p>
          <p class="uk-article-meta uk-text-center" v-else>Likes: 0 <span uk-icon="icon: heart" class="uk-padding-small uk-padding-remove-left"></span></p>

          <div class="uk-text-left">
            <p v-if="post.content" id="editor" v-html="$md.render(post.content)"></p>
          </div>

          <div class="uk-grid-small uk-child-width-auto" uk-grid>
            <div>
              <a class="uk-button uk-button-text" href="#" v-if="post.comments"> {{ post.comments.length }} Comments</a>
            </div>
          </div>

          <hr class="uk-margin-medium">
        </article>

        <div class="uk-position-relative uk-margin-medium uk-position-center">
          <div class="uk-margin" v-if="this.$auth.loggedIn">
            <ul class="uk-iconnav" v-if="!this.show_comment_box">
              <li><button class="uk-button uk-button-secondary" @click="show_comment_box = !show_comment_box" type="button">Post a Comment!</button></li>
            </ul>
          </div>
        </div>

        <div v-if="this.show_comment_box" class="uk-container uk-position-relative">
          <div class="uk-position-relative uk-margin-medium">
            <form>
              <div v-if="this.show_alert" class="uk-alert-danger" uk-alert>
                <a class="uk-alert-close" uk-close></a>
                <p>You must be logged in to comment.</p>
              </div>
              <fieldset class="uk-fieldset">
                <div class="uk-margin">
                  <client-only>
                    <vue-simplemde v-model="comment_body" ref="markdownEditor" />
                  </client-only>
                </div>
                <div class="uk-margin" v-if="this.$auth.loggedIn">
                  <button class="uk-button uk-button-secondary" @click="postComment()" type="submit">Post Comment</button>
                </div>
              </fieldset>
            </form>
          </div>
        </div>

        <!-- Comment section -->
        <div class="uk-position-relative uk-margin-medium uk-text-left">
          <div class="uk-section-muted uk-margin-medium"  v-for="comment in post.comments">
            <div class="uk-padding">
              <article class="uk-comment">
                  <header class="uk-comment-header uk-grid-medium uk-flex-middle" uk-grid>
                      <div class="uk-width-expand">
                          <h4 class="uk-comment-title uk-margin-remove"><a class="uk-link-reset" href="#">{{ comment.user.username }}</a></h4>
                          <ul class="uk-comment-meta uk-subnav uk-subnav-divider uk-margin-remove-top">
                              <li><a href="#">{{moment(comment.created_at).format("LLL")}}</a></li>
                              <div class="uk-float-right" v-if="$auth.user.role.name === 'Moderator' || $auth.user.role.name === 'Admin'">
                                <a @click="deleteComment(comment.id)"  uk-icon="icon: close">Delete Comment</a>
                              </div>
                          </ul>
                          <hr class="uk-margin-small">
                      </div>
                  </header>
                  <div class="uk-comment-body">
                      <p v-html="$md.render(comment.content)"></p>
                  </div>
                  <br>
              </article>
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
      show_success: false

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
    },
    likePost() {
      var liked = this.post.like_count + 1;
      axios
        .put('http://localhost:1337/posts/' + this.post.id, {
          like_count: liked
        })
        .then(response => {
          // Handle success.
          console.log('Successfully Liked Comment.');
          this.show_success = true;
          this.$router.push('/posts/' + this.post.id);
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
          this.show_alert = true;
        });
    },
    deleteComment(id) {
      axios
        .delete('http://localhost:1337/comments/' + id)
        .then(response => {
          console.log('Comment Deleted.');
        })
        .catch(error => {
          console.log('An error has occurred:', error);
        });
    }
  }
}
</script>
