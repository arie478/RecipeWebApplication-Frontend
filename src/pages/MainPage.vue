<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <div class="float-child">
    <RecipeViewerList title="Random Recipes" :isPreview="true" :show_ing_and_serv="false" :recipes="this.recipes" class="RandomRecipes center"/>
    </div>

    <!-- <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link> -->
    <div class="float-child">
    <LoginPage v-if="!$root.store.username"/>
    </div>
    <!-- {{ !$root.store.username }} -->
    <div class="float-child">
    <RecipeViewerList
      title="Last Viewed Recipes"
      :class="{
        RandomRecipes: true,
        blur: !$root.store.username,
        center: true
      }"
      :isPreview="true"
      disabled
    v-if="$root.store.username"></RecipeViewerList>
    </div>

    <!-- <div
       width: 50%;
       float: left; style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
  </div>
</template>

<script>
import RecipeViewerList from "../components/RecipeViewerList";
import LoginPage from "./LoginPage.vue";
export default {
  components: {
    RecipeViewerList,
    LoginPage
},

mounted() {
    this.updateRandomRecipes();
  },
   data() {
    return {
      recipes: []
    };
  },
  methods: {
    async updateRandomRecipes() {
      try {
        // const response = await this.axios.get(
        //   // this.$root.store.server_domain + "/recipes/random",
        //   // "https://test-for-3-2.herokuapp.com/recipes/random"
        //   "http://localhost:3000/recipes/random",
        // );

        this.recipes = [];
        //this.recipes.push(...response.data);
      } catch (error) {
        console.log(error);
      }
    }
  }

};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

.float-child 
{
    width: 50%;
    float: left;
}  

</style>
