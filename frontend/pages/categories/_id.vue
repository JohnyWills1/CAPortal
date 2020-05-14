<template>
  <div>

    <client-only>
    <div class="uk-section">
      <div class="uk-container uk-container-large">
        <h1>{{ category.name }}</h1>

        <Posts :posts="category.posts || []"></Posts>

      </div>
    </div>
  </client-only>
  </div>
</template>

<script>
import postsQuery from '~/apollo/queries/post/posts-categories'
import Posts from '~/components/Posts'

export default {
  data() {
    return {
      category: []
    }
  },
  components: {
    Posts
  },
  apollo: {
    category: {
      prefetch: true,
      query: postsQuery,
      variables () {
        return { id: parseInt(this.$route.params.id) }
      }
    }
  }
}
</script>
