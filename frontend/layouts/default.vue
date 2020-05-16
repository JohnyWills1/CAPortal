<template>
<div>
  <nav class="uk-navbar-container" uk-navbar>
    <div class="uk-navbar-left">
      <ul class="uk-navbar-nav">
        <li><a href="#modal-offnav" uk-toggle><span uk-icon="table"></span></a></li>
        <li>
          <NuxtLink to="/"><span uk-icon="home"></span></NuxtLink>
        </li>
      </ul>
    </div>


    <div class="uk-navbar-right" v-if="$auth.loggedIn">
      <ul class="uk-navbar-nav">
        <li><a><span class="uk-padding-small" uk-icon="user"></span>{{ $auth.user.username }}</a></li>
        <li><button class="uk-button uk-button-secondary uk-button-small" v-on:click="logout()"><span class="uk-padding-small" uk-icon="settings"></span>Logout</button></li>
      </ul>
    </div>

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
              <router-link class="uk-modal-close" :to="{ name: 'threads-id', params: { id: thread.id }}" tag="a">{{ thread.name }}</router-link>
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
  async logout() {
    try {
      this.$auth.logout()
      this.$axios.setHeader('Authorization', null)
      this.$router.push("/")
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>
