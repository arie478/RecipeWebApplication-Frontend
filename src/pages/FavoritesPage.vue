<template>
  <div class="container">
    <RecipeViewerList 
    title="Favorites Page"
    :recipes="favoritesRecipes"
    :isPreview="true"
    :show_ing_and_serv="false"
    />
  </div>
</template>

<script>
import RecipeViewerList from '../components/RecipeViewerList.vue';

export default
  {
    name: "FavoritesPage",
    components:
    {
      RecipeViewerList: RecipeViewerList
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