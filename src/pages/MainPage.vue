<template>

  <div class="container">

    <!-- <h1 class="title">Main Page</h1> -->

    <div class="float-child">
      <RecipeViewerList title="Explore this Recipes" :recipes="randomRecipes" :isPreview="true" :show_ing_and_serv="false" />
    </div>

      <div class="float-child">
        <LoginPage v-if="!$root.store.username" />
      </div>

    <div class="float-child">
      <RecipeViewerList v-if="this.$root.store.username" title="Last Watched Recipes" :recipes="lastViewedRecipes" :isPreview="true" :show_ing_and_serv="false" disabled />
    </div>

  </div>
</template>

<script>

import RecipeViewerList from "../components/RecipeViewerList";
import LoginPage from "./LoginPage.vue";

export default 
{
  components:
  {
    RecipeViewerList,
    LoginPage
  },

  data() {
    return {
      randomRecipes: [],
      lastViewedRecipes: []
    };
  },

  methods:
  {
    async updateRandomRecipes() {
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          "http://localhost:3000/recipes/random",
        );
        this.axios.defaults.withCredentials = false;

        console.log(response);
        this.randomRecipes = [];
        this.randomRecipes.push(...response.data);
      }
      catch (err) {
        console.log(err.response);
      }
    },

    async updateLastViewedRecipes() {
      if (this.$root.store.username) {
        try {
          this.axios.defaults.withCredentials = true;
          const response = await this.axios.get(
            "http://localhost:3000/users/getLastWatched",
          );
          this.axios.defaults.withCredentials = false;

          console.log(response);
          this.lastViewedRecipes = [];
          this.lastViewedRecipes.push(...response.data);
        }
        catch (err) {
          console.log(err.response);
        }
      }
    }
  },

  mounted() {
    this.updateRandomRecipes();
    this.updateLastViewedRecipes();
  },
};
</script>

<style lang="scss" scoped>
// .RandomRecipes {
//   margin: 10px 0 10px;
// }

.blur {
  -webkit-filter: blur(5px);
  /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}

::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

.float-child {
  width: 50%;
  float: left;
}
</style>
