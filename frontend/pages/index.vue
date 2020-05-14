<template>
  <div>

    <div class="uk-section">
      <div class="uk-container uk-container-large">
        <h1>CA-Portal</h1>

        <Posts :posts="posts"></Posts>

      </div>
    </div>

  </div>
</template>
<nuxt/>

<script>
import postsQuery from '~/apollo/queries/post/posts'
import Posts from '~/components/Posts'

export default {
  data() {
    return {
      posts: [],
    }
  },
  components: {
    Posts
  },
  apollo: {
    posts: {
      prefetch: true,
      query: postsQuery,
      variables () {
        return { id: parseInt(this.$route.params.id) }
      }
    }
  }
}
</script>
