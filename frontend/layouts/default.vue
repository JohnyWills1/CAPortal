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

    <div class="uk-navbar-right">
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
            <li v-for="category in categories">
              <router-link class="uk-modal-close" :to="{ name: 'categories-id', params: { id: category.id }}" tag="a">{{ category.name }}</router-link>
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
import categoriesQuery from '~/apollo/queries/category/categories'

export default {
  data() {
    return {

    }
  },
  apollo: {
    categories: {
      prefetch: true,
      query: categoriesQuery
    }
  },
  methods: {

  }
}
</script>
