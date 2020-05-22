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
          <!-- Admin Dashboard -->
          <nuxt-link class="uk-navbar-item" to="/adminDashboard">
            <div v-if="$auth.loggedIn">
              <div v-if="$auth.user.role.name === 'Admin' ">
                <span class="uk-padding-small" uk-icon="bolt"></span>
                Admin Dashboard
              </div>
            </div>
          </nuxt-link>
        </li>
        <li>
          <nuxt-link :to="{ name: 'user-id', params: { id: $auth.user.id }}" class="uk-link-reset uk-navbar-item" :key="$auth.user.id">
            <span class="uk-padding-small" uk-icon="settings"></span>
            {{ $auth.user.username }}
          </nuxt-link>
        </li>
        <li>
          <a class="uk-navbar-item" @click.prevent="logout()">
            <span class="uk-padding-small" uk-icon="sign-out"></span>
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
              <div v-if="thread.admin_accepted">
                <router-link class="uk-modal-close" :to="{ name: 'thread-id', params: { id: thread.id }}" tag="a">{{ thread.name }}</router-link>
              </div>
            </li>
          </ul>
      </div>
    </div>
  </div>

  <!-- CAPortal Content inserted here -->
  <nuxt />

  <!-- Footer section -->
  <div class="uk-section uk-section-muted">
    <p class="uk-text-center">Website built with:</p>
    <div class="uk-container uk-container-center uk-text-center">
      <a href="https://graphql.org/">
        <img src="http://localhost:1337/uploads/b8bef38fd40142238ac8fcd0b8717e91.svg" width="50" height="50" uk-svg>
      </a>
      <a href="https://nuxtjs.org/">
        <img src="http://localhost:1337/uploads/6fd2c55afcc348a7a7638201271a8726.svg" width="50" height="50" uk-svg>
      </a>
      <a href="https://strapi.io/">
        <img src="http://localhost:1337/uploads/cbf142b61beb43b395ee645a12ef73d6.svg" width="50" height="50" uk-svg>
      </a>
      <a href="https://getuikit.com/">
        <img src="http://localhost:1337/uploads/cdb69092c6b14a7db9d3d63908366d1b.svg" width="50" height="50" uk-svg>
      </a>
    </div>
  </div>


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
