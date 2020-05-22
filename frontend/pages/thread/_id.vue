<template>
<div>
  <div class="uk-section">
    <div class="uk-container uk-position-relative">
      <h1 class="uk-heading-large">{{thread.name}}</h1>
    </div>
    <div class="uk-container uk-container-small uk-position-relative">
      <div v-if="thread.posts && thread.posts.length">
        <div class="uk-position-relative uk-margin-medium">
        </div>
        <hr class="uk-margin-medium">
        <div class="uk-alert-danger" v-if="this.show_alert" uk-alert>
          <a class="uk-alert-close" uk-close></a>
          <p>Failed to submit Post, either you are not logged in or do not have permissions.</p>
        </div>
        <div class="uk-alert-success" v-if="this.show_success" uk-alert>
          <a class="uk-alert-close" uk-close></a>
          <p>Successfully submitted post in: {{ thread.name }}</p>
        </div>

        <div class="uk-padding" v-for="post in thread.posts">
          <router-link :to="{ name: 'posts-id', params: { id: post.id }}" :key="post.id">
            <div class="uk-container uk-padding-xsmall">
              <ul class="uk-list uk-list-striped">
                <li v-if="post.title" class="uk-">
                  {{ post.title }}
                  <div class="uk-text-right">
                    <span class="uk-padding-small" uk-icon="user"></span>{{ post.user.username }}
                    <span class="uk-padding-small" uk-icon="commenting"></span>{{ post.comments.length }}
                    <span class="uk-padding-small" uk-icon="heart"></span>{{ post.like_count }}
                  </div>
                </li>
              </ul>
            </div>
          </router-link>
          <div class="uk-float-right" v-if="$auth.user.role.name === 'Moderator' || $auth.user.role.name === 'Admin'">
            <a @click="deletePost(post.id)"  uk-icon="icon: close" class="uk-padding">Delete Post</a>
          </div>
        </div>

      </div>

      <div class="uk-section" v-else>
        <div class="uk-container uk-text-center">
          <hr class="uk-margin-medium">
          <div>No Posts in this Topic yet...</div>
        </div>
      </div>

      <div class="uk-position-top-right uk-margin-bottom" v-if="this.$auth.loggedIn">
        <ul class="uk-iconnav">
          <li><button class="uk-button uk-button-secondary" uk-toggle="target: #post_modal" type="button">Create a Post!</button></li>
        </ul>
      </div>



    </div>
  </div>

  <!-- Create post modal -->
  <div id="post_modal" class="uk-modal" uk-modal>
    <div class="uk-modal-dialog uk-modal-body">
      <button class="uk-modal-close-default" type="button" uk-close></button>
      <form @submit.prevent="createPost()">
        <fieldset class="uk-fieldset">

            <h1 class="uk-heading-medium uk-text-center">Create a Post</h1>
            <hr class="uk-margin-medium">
            <div class="uk-margin uk-text-center">
              <input class="uk-input uk-form-width-large" type="text" placeholder="Post Title" ref="post_title">

              <div class="uk-margin">
                <textarea class="uk-textarea" rows="5" placeholder="Post Body" ref="post_body"></textarea>
              </div>

              <div class="uk-margin uk-text-center">
                <button class="uk-button uk-button-primary uk-button-large uk-width-1-1" uk-toggle="target: #post_modal" type="submit">Submit</button>
              </div>
            </div>
        </fieldset>
      </form>
    </div>
  </div>
</div>
</template>

<script>
import thread_postsQuery from '~/apollo/queries/thread/thread_posts'
import axios from 'axios';

var moment = require('moment')

export default {
  data() {
    return {
      thread: {},
      post_title: '',
      post_body: '',
      moment: moment,
      show_alert: false,
      show_success: false,
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
  },
  methods: {
    createPost() {

      //Only using refs to get input data as vue bindings cause the modal to close on keyboard event
      var post = {
        title: this.post_title = this.$refs.post_title.value,
        content: this.post_body = this.$refs.post_body.value,
        user: this.$auth.user
      }

      axios
        .post('http://localhost:1337/posts', {
          title: this.post_title,
          content: this.post_body,
          user: this.$auth.user
        })
        .then(response => {
          // Handle success.
          console.log('Successfully created Post.');
          axios
            .put('http://localhost:1337/threads/' + this.thread.id, {
              posts: this.thread.posts.concat(response.data)
            })
            .then(response => {
              console.log("Successfully added post to this thread.")
              this.show_success = true;
              this.$router.push('/thread/' + this.thread.id)
            })
            .catch(error => {
              console.log('An error occured:', error);
              this.show_alert = true;
            });
          this.show_success = true;
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
          this.show_alert = true;
        });
    },
    deletePost(id) {
      axios
        .delete('http://localhost:1337/posts/' + id)
        .then(response => {
          console.log('Post Deleted.');
        })
        .catch(error => {
          console.log('An error has occurred:', error);
        });
    }
  }
}
</script>
