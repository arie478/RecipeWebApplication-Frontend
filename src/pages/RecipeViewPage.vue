<template>
  <div class="container">
    <div v-if="recipe">
      <br>
     <b-card
        border-variant="secondary"
         header-border-variant="secondary"
        :header=  recipe.title
        align="center"
      >
        <b-card-text>
      <div class="recipe-header mt-3 mb-4">
       
        <img :src="recipe.image" class="center" />
  
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">

            <b-card border-variant="info" header="Usefull information" align="center">
                            <b-card-text>
            <div class = "mb-3">
               <b-row>
                  <b-col v-if="recipe.glutenFree" >
                    <b-img src="../assets/gluten_free_text.png" height="50%" width="50%"></b-img>
                  </b-col>
                  <b-col v-if="!recipe.glutenFree" >
                    <b-img src="../assets/gluten_text.png" height="50%" width="50%"></b-img>
                  </b-col>
                  <b-col v-if="recipe.vegetarian">
                    <b-img src="../assets/vegetarian_text.png" height="50%" width="50"></b-img>
                  </b-col>
                  <b-col v-if="recipe.began">
                    <b-img src="../assets/vegan_text.png" height="50%" width="50%"></b-img>
                  </b-col>

                  <b-col v-if="$root.store.username && this.$route.params.personal != 'personal'">
                    <b-button :disabled="this.recipe.is_favoried" @click="add_to_favorite" variant="outline-warning" class="mb-2">
                      <b-icon  :icon="this.change_favorite()"></b-icon>
                    </b-button>
                  </b-col> 
                  <b-col v-if="$root.store.username && this.$route.params.personal != 'personal'">
                    <b-icon  :icon="this.change_watched()" scale="2" shift-v="-8"></b-icon>
                  </b-col>

                </b-row>
            </div>

            <div class="mb-3">
               
                  <div> Ready in  {{ recipe.readyInMinutes }} minutes</div>
                  <div> {{ recipe.aggregateLikes }} people liked this recipe</div>
                  <div> Makes {{ recipe.servings }} servings </div>
            </div>
                </b-card-text>
              </b-card>

            <br>

            <b-card
              border-variant="secondary"
              header="Ingredients"
              header-border-variant="secondary"
              align="center"
              >
            <b-card-text align="left">
                <ul>
                  <li
                    v-for="(r, index) in recipe.ingredients.flat()"
                    :key="index + '_' + r.id"
                  >
                    {{ r }}
                  </li>
                </ul>
            </b-card-text>
            </b-card>
          </div>
          <div class="wrapped">
             <b-card
              border-variant="secondary"
              header="Instructions"
              header-border-variant="secondary"
              align="center"
              >
                <b-card-text align="left">
                  <ol>
                    <li v-for="s in recipe.instructions.flat()" :key="s">
                      {{ s }}
                    </li>
                  </ol>
                </b-card-text>
              </b-card>
          </div>
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
      </b-card-text>
      </b-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null
    };
  },
  methods: 
  {
async add_to_favorite() 
        {
          if(this.recipe.is_favoried)
          {
            return false;
          }
          else
          {
          try 
          {
              this.axios.defaults.withCredentials = true;
              const response = await this.axios.post(
              //   // this.$root.store.server_domain + "/recipes/random",
              //   // "https://test-for-3-2.herokuapp.com/recipes/random"
                "http://localhost:3000/users/addToFavorites",
                  {
                    recipeId: this.recipe.id
                  }
              );
              this.axios.defaults.withCredentials = false;
              console.log(response.data);


              const recipes = JSON.parse(sessionStorage.search);

              for (var i = 0; i < recipes.length; i ++)
              {
                //console.log("checking id");
                //console.log(recipes[i].id);
                //console.log("looking for");
                //console.log(this.recipe.id);

                if (recipes[i].id == this.recipe.id)
                {
                  recipes[i].is_favoried = true;
                  //console.log(recipes[i].is_favoried);
                  sessionStorage.search = JSON.stringify(recipes);
                  //console.log(sessionStorage.search);

                }
              }

              
              
            } 
            catch (error) 
            {
                  console.log(error);
            }

            this.recipe.is_favoried = true;
          }
          this.$forceUpdate();
        },

        change_favorite() 
        {
          if(this.recipe.is_favoried)
          {
            return "star-fill";
          }
          else
          {
            return "star";
          }
        },

        change_watched() 
        {
          if(this.recipe.is_watched)
          {
            return "eye-fill";
          }
          else
          {
            return "eye-slash";
          }
        },
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;

      console.log(this.$route.params);
      console.log(this.$route.params.personal);

      if (this.$route.params.personal == "personal")
      {
          this.axios.defaults.withCredentials = true;
          response = await this.axios.get(
            // "https://test-for-3-2.herokuapp.com/recipes/info",
            // this.$root.store.server_domain + "/recipes/info",
            "http://localhost:3000/users/getFullPersonalRecipes/" + this.$route.params.recipeId,
            // {
            //   params: { recipeId: this.$route.params.recipeId }
            // }
          );
           this.axios.defaults.withCredentials = false;

          console.log("response.status", response.status);
          if (response.status !== 200) this.$router.replace("/NotFound");

          console.log("Personal recipe responde:");
          console.log(response.data);
          this.recipe = response.data[0];
          console.log(this.recipe);
      }
      else
      {

        try {
          this.axios.defaults.withCredentials = true;
          response = await this.axios.get(
            // "https://test-for-3-2.herokuapp.com/recipes/info",
            // this.$root.store.server_domain + "/recipes/info",
            "http://localhost:3000/recipes/" + this.$route.params.recipeId,
            // {
            //   params: { recipeId: this.$route.params.recipeId }
            // }
          );
          this.axios.defaults.withCredentials = false;

          console.log("response.status", response.status);
          if (response.status !== 200) this.$router.replace("/NotFound");

          this.recipe = response.data;
          console.log(this.recipe);

        } catch (error) {
          console.log("error.response.status", error.response.status);
          this.$router.replace("/NotFound");
          return;
        }
      }


      // let {
      //   analyzedInstructions,
      //   instructions,
      //   extendedIngredients,
      //   aggregateLikes,
      //   readyInMinutes,
      //   image,
      //   title
      // } = response.data.recipe;

      // let _instructions = analyzedInstructions
      //   .map((fstep) => {
      //     fstep.steps[0].step = fstep.name + fstep.steps[0].step;
      //     return fstep.steps;
      //   })
      //   .reduce((a, b) => [...a, ...b], []);

      // let _recipe = {
      //   instructions,
      //   _instructions,
      //   analyzedInstructions,
      //   extendedIngredients,
      //   aggregateLikes,
      //   readyInMinutes,
      //   image,
      //   title
      // };

      // this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}
.wrapped {
  width: 50%;
  margin-left: 1%;
  
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
