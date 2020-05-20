<template>
<div>
  <div class="uk-section">
    <div class="uk-container">
      <h1 class="uk-heading-large">{{thread.name}}</h1>
    </div>
    <div class="uk-container uk-container-small uk-position-relative">

      <div v-if="thread.posts && thread.posts.length">
        <div class="uk-position-relative uk-margin-medium">
        </div>
        <hr class="uk-margin-medium">
        <router-link class v-for="post in thread.posts" :to="{ name: 'posts-id', params: { id: post.id }}" :key="post.id">
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
        </router-link>
      </div>

      <div class="uk-section" v-else>
        <div class="uk-container uk-text-center">
          <hr class="uk-margin-medium">
          <div>No Posts in this Topic yet...</div>
        </div>
      </div>

      <div class="uk-position-top-right uk-margin-bottom">
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
              <input class="uk-input uk-form-width-large" type="text" placeholder="Post Title">

              <div class="uk-margin">
                <textarea class="uk-textarea" rows="5" placeholder="Post Body"></textarea>
              </div>

              <div class="uk-margin">
                <input class="uk-input uk-form-width-large" type="text" placeholder="Tags: seperate with commas - food,travel,tech">
              </div>

              <div class="uk-margin uk-text-center">
                <button class="uk-button uk-button-primary uk-button-large uk-width-1-1" type="submit">Submit</button>
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

var moment = require('moment')

export default {
  data() {
    return {
      thread: {},
      post_title: '',
      post_body: '',
      post_tags: [],
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
  },
  methods: {
    createPost() {

    }
  }
}
</script>
