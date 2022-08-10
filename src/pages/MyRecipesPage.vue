<template>
  <div class="container">
    <RecipeList 
    title="My Recipes Page"
    :recipes="myRecipes"
    :isPreview="true"
    />
  </div>
</template>

<script>
import RecipeList from '../components/RecipeList.vue';

export default
  {
    name: "MyRecipesPage",
    components:
    {
      RecipeList: RecipeList
    },
    data() {
      return {
        myRecipes: []
      };
    },
    methods:
    {
      async getMyRecipes() {
        try {
          this.axios.defaults.withCredentials = true;
          const response = await this.axios.get(
            "http://localhost:3000/users/getPreviewPersonalRecipes"
          );
          this.axios.defaults.withCredentials = false;

          console.log(response);
          this.myRecipes = [];
          this.myRecipes.push(...response.data);
        }
        catch (err) {
          console.log(err.response);
        }
      }
    },
    mounted() {
      this.getMyRecipes();
    }
  };
</script>

<style lang="scss" scoped>
.container{
  max-width: 520px;
}
</style>