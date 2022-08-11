<template>
  <div class="container">
    <RecipeViewerList 
    title="Family Recipes Page"
    :recipes="familyRecipes"
    :isPreview="true"
    :show_ing_and_serv="false"
    />
  </div>
</template>

<script>
import RecipeViewerList from '../components/RecipeViewerList.vue';

export default
  {
    name: "FamilyRecipesPage",
    components:
    {
      RecipeViewerList: RecipeViewerList
    },
    data() {
      return {
        familyRecipes: []
      };
    },
    methods:
    {
      async getFamilyRecipes() {
        try {
          this.axios.defaults.withCredentials = true;
          const response = await this.axios.get(
            "http://localhost:3000/users/getFamilyRecipes"
          );
          this.axios.defaults.withCredentials = false;

          console.log(response);
          this.familyRecipes = [];
          this.familyRecipes.push(...response.data);
        }
        catch (err) {
          console.log(err.response);
        }
      }
    },
    mounted() {
      this.getFamilyRecipes();
    }
  };
</script>

<style lang="scss" scoped>
.container{
  max-width: 520px;
}
</style>