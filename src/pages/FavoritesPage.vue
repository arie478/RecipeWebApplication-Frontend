<template>
  <div class="container">
    <RecipeList 
    title="Favorites Page"
    :recipes="favoritesRecipes"
    :isPreview="true"
    />
  </div>
</template>

<script>
import RecipeList from '../components/RecipeList.vue';

export default
  {
    name: "FavoritesPage",
    components:
    {
      RecipeList: RecipeList
    },
    data() {
      return {
        favoritesRecipes: []
      };
    },
    methods:
    {
      async getFavoritesRecipes() {
        try {
          this.axios.defaults.withCredentials = true;
          const response = await this.axios.get(
            "http://localhost:3000/users/getFavorites"
          );
          this.axios.defaults.withCredentials = false;

          console.log(response);
          this.favoritesRecipes = [];
          this.favoritesRecipes.push(...response.data);
        }
        catch (err) {
          console.log(err.response);
        }
      }
    },
    mounted() {
      this.getFavoritesRecipes();
    }
  };
</script>

<style lang="scss" scoped>
.container{
  max-width: 520px;
}
</style>