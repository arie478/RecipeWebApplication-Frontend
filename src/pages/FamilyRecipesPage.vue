<template>
  <div class="container">
    <RecipeList 
    title="Family Recipes Page"
    :recipes="familyRecipes"
    :isPreview="true"
    />
  </div>
</template>

<script>
import RecipeList from '../components/RecipeList.vue';

export default
  {
    name: "FamilyRecipesPage",
    components:
    {
      RecipeList: RecipeList
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