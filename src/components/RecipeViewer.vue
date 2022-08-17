<template>
  <div class="recipe-box">
    <b-card border-variant="secondary" header-border-variant="secondary" align="center">
      <b-card-text>
        <div class="recipe-preview">
          <router-link v-if="personal == true" :to="{ path: '/recipe/' + recipe.id + '/personal' }">
            <div class="recipe-body">
              <img :src="recipe.image" class="recipe-image" />
            </div>
            <div class="recipe-footer">
              <div :title="recipe.title" class="recipe-title">
                <h4>{{ recipe.title }}</h4>
              </div>

            </div>
          </router-link>

          <router-link v-if="personal == false" :to="{ path: '/recipe/' + recipe.id }">
            <div class="recipe-body">
              <img :src="recipe.image" class="recipe-image" />
            </div>
            <div class="recipe-footer">
              <div :title="recipe.title" class="recipe-title">
                <h4>{{ recipe.title }}</h4>
              </div>

            </div>
          </router-link>

          <b-card border-variant="info" header="Usefull information" align="center">
            <b-card-text>
              <div class="recipe-markings mb-3">
                <b-row>
                  <b-col v-if="recipe.glutenFree">
                    <b-img src="../assets/gluten_free_text.png" height="50%" width="50%"></b-img>
                  </b-col>
                  <b-col v-if="!recipe.glutenFree">
                    <b-img src="../assets/gluten_text.png" height="50%" width="50%"></b-img>
                  </b-col>
                  <b-col v-if="recipe.vegetarian">
                    <b-img src="../assets/vegetarian_text.png" height="50%" width="50"></b-img>
                  </b-col>
                  <b-col v-if="recipe.began">
                    <b-img src="../assets/vegan_text.png" height="50%" width="50%"></b-img>
                  </b-col>
                </b-row>

                <br>
                <b-row v-if="$root.store.username && personal == false">
                  <b-col>
                    <b-button :disabled="this.recipe.is_favoried" @click="add_to_favorite" variant="outline-warning"
                      class="mb-2">
                      <b-icon :icon="this.change_favorite()"></b-icon>
                    </b-button>
                  </b-col>
                  <b-col>
                    <b-icon :icon="this.change_watched()" scale="2" shift-v="-8"></b-icon>
                  </b-col>
                </b-row>
              </div>

              <br>

              <div>
                <b-row class="recipe-overview">
                  <b-col>
                    <h5>Time to make:</h5> {{ recipe.readyInMinutes }} minutes
                  </b-col>
                  <b-col>
                    <h5>Popularity:</h5> {{ recipe.aggregateLikes }} likes
                  </b-col>
                  <b-col v-if="!isPreview && show_ing_and_serv">
                    <h5>Servings:</h5> {{ recipe.servings }}
                  </b-col>
                </b-row>
              </div>

            </b-card-text>
          </b-card>

          <br>

          <div v-if="!isPreview">

            <b-col v-if="show_ing_and_serv">
              <br>
              <h5>Ingredients:</h5>
              <b-row v-for="ingredient in recipe.ingredients.flat()" :key="ingredient">
                <li>{{ instruction }}</li>

              </b-row>
            </b-col>

            <b-card border-variant="secondary" header="Instructions" header-border-variant="secondary" align="center">
              <b-card-text align="left">
                <b-col>
                  <br>
                  <b-row v-for="instruction in recipe.instructions.flat()" :key="instruction">
                    <li>{{ instruction }}</li>
                  </b-row>
                </b-col>
              </b-card-text>
            </b-card>
          </div>
        </div>
      </b-card-text>
    </b-card>
  </div>
</template>

<script>
export default {
  methods:
  {
    async add_to_favorite() {
      if (this.recipe.is_favoried) {
        return false;
      }
      else {
        try {
          this.axios.defaults.withCredentials = true;
          const response = await this.axios.post(
            //   // this.$root.store.server_domain + "/recipes/random",
            //   // "https://test-for-3-2.herokuapp.com/recipes/random"
            // "http://localhost:3000/users/addToFavorites",
            "https://foodisgood.cs.bgu.ac.il/users/addToFavorites",
            {
              recipeId: this.recipe.id
            }
          );
          this.axios.defaults.withCredentials = false;
          console.log(response.data);

          const recipes = JSON.parse(sessionStorage.search);

          for (var i = 0; i < recipes.length; i++) {
            if (recipes[i].id == this.recipe.id) {
              recipes[i].is_favoried = true;
              sessionStorage.search = JSON.stringify(recipes);
            }
          }
        }
        catch (error) {
          console.log(error);
        }
        this.recipe.is_favoried = true;
      }
      this.$forceUpdate();
    },

    change_favorite() {
      if (this.recipe.is_favoried) {
        return "star-fill";
      }
      else {
        return "star";
      }
    },

    change_watched() {
      if (this.recipe.is_watched) {
        return "eye-fill";
      }
      else {
        return "eye-slash";
      }
    },
  },

  props: {
    recipe: {
      type: Object,
      required: true
    },
    show_ing_and_serv: {
      type: Boolean,
      required: true
    },
    isPreview:
    {
      type: Boolean,
      required: true
    },
    personal:
    {
      type: Boolean,
      required: false
    }
  }
};
</script>

<style scoped>
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}

.recipe-preview>.recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 100%;
  height: 100%;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  overflow: hidden;
}

.recipe-preview .recipe-footer {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: center;
  white-space: initial;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
  text-decoration: underline;
}

h4,
h5 {
  text-decoration: underline;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

recipe-overview {
  text-align: center;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90vw;
  display: table-cell;
  text-align: center;
}

.recipe-box
{
  --b: 10px; /* control the size */
  width: 100%;
  padding: var(--b);
}

li {
  margin-top: 15px;
}

.recipe-markings {
  margin: auto;
  width: 50%;
  text-align: center;
}

.recipe-overview {
  margin: auto;
  width: 100%;
  text-align: center;
}
</style>