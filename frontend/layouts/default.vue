<template>
<div>
  <nav class="uk-navbar-container uk-margin uk-flex" uk-navbar>
    <!-- Left Nav Bar -->
    <div class="uk-navbar-left">
      <ul class="uk-navbar-nav">
        <li>
          <a class="uk-navbar-toggle" href="#modal-offnav" uk-toggle>
              <span class="uk-padding-small" uk-icon="menu"></span>
              Menu
          </a>
        </li>
      </ul>
    </div>

    <div>
      <ul class="uk-navbar-nav uk-flex-center">
        <li>
          <a class="uk-logo" href="/">CA<span class="uk-icon" uk-icon="icon: world"></span>Portal</a>
        </li>
      </ul>
    </div>

    <!-- If user is logged in then load the account -->
    <div class="uk-navbar-right" v-if="$auth.loggedIn">
      <ul class="uk-navbar-nav">
        <li>
          <a class="uk-navbar-item">
            <span class="uk-padding-small" uk-icon="user"></span>
            {{ $auth.user.username }}
          </a>
        </li>
        <li>
          <a class="uk-navbar-item" @click.prevent="logout()">
            <span class="uk-padding-small" uk-icon="settings"></span>
            Logout
          </a>
        </li>
      </ul>
    </div>
    <!-- normal right nav bar no user logged in  -->
    <div class="uk-navbar-right" v-else>
      <ul class="uk-navbar-nav">
        <li><a href="/login" uk-toggle><span class="uk-padding-small" uk-icon="user"></span>Login</a></li>
        <li><a href="/signup" uk-toggle><span class="uk-padding-small" uk-icon="sign-in"></span>Sign-up</a></li>
      </ul>
    </div>
  </nav>

  <div id="modal-offnav" uk-offcanvas="overlay: true">
    <div class="uk-offcanvas-bar uk-flex uk-flex-column">
      <div class="uk-offcanvas-content">
        <button class="uk-offcanvas-close" type="button" uk-close></button>
          <h2>CA-Portal</h2>
          <ul class="uk-nav uk-nav-primary uk-nav-parent-icon" uk-nav>
            <li v-for="thread in threads">
              <router-link class="uk-modal-close" :to="{ name: 'thread-id', params: { id: thread.id }}" tag="a">{{ thread.name }}</router-link>
            </li>
          </ul>
        <p class="uk-text-light">Built with Strapi</p>
      </div>
    </div>
  </div>

  <nuxt />
</div>
</template>

<script>
import threadsQuery from '~/apollo/queries/thread/threads';
import axios from 'axios';

export default {
  data() {
    return {

    }
  },
  apollo: {
    threads: {
      prefetch: true,
      query: threadsQuery
    }
  },
  methods: {
    logout() {
      console.log("Logged Out, Bye :D")
      this.$auth.reset()
    }
  }
}
</script>
