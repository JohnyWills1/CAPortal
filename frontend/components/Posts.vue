  <template>
  <div>
    <div class="uk-text-center uk-grid-match uk-child-width-expand@s" uk-grid>
      <router-link v-for="category in categories" :to="{ name: 'categories-id', params: { id: category.id }}" class="uk-link-reset" :key="category.id">
        <div class="uk-card uk-card-default">
          <div class="uk-card-media-top">
            <img :src="api_url + category.Image[0].url" :alt="category.name" width="400" height="400">
          </div>
          <div class="uk-card-body">
            <div class="uk-card-badge uk-label">{{ category.name }}</div>
          </div>
       </div>
      </router-link>
    </div>
  </div>
</template>

<script>
import categoriesQuery from '~/apollo/queries/category/categories'

export default {
  data() {
    return {
      api_url: process.env.strapiBaseUri,
      categories: []
    }
  },
  apollo: {
    categories: {
      prefetch: true,
      query: categoriesQuery
    }
  }
}
</script>
