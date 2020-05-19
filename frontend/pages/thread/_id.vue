<template>
<div>
  <div class="uk-section">
    <div class="uk-container uk-container-small uk-position-relative">
      <div class="uk-position-relative uk-margin-medium">
        <div class="uk-position-top-right uk-margin-bottom">
          <ul class="uk-iconnav">
            <li><button class="uk-button uk-button-primary" uk-toggle="target: #post_modal" type="button">Create a Post!</button></li>
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

  <!-- Create post modal -->
  <div id="post_modal" class="uk-modal" uk-modal>
    <div class="uk-modal-dialog uk-modal-body">
      <button class="uk-modal-close-default" type="button" uk-close></button>
      <form @submit.prevent="createThread()">
        <fieldset class="uk-fieldset">

            <h1 class="uk-heading-medium uk-text-center">Create a Post</h1>
            <hr class="uk-margin-medium">
            <div class="uk-margin uk-text-center">
              <input class="uk-input uk-form-width-large" type="text" placeholder="Post Title">


              <hr class="uk-margin-medium">

              <div class="uk-margin">
                <textarea class="uk-textarea" rows="5" placeholder="Post Body"></textarea>
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
