<template>
  <div class="container">
    <h1 class="title">Search</h1>
    <b-form @submit.prevent="onSearch" @reset.prevent="onReset">
      <b-form-group
        id="input-group-recipename"
        label-cols-sm="3"
        label="Recipe Name:"
        label-for="recipename"
      >
        <b-form-input
          id="recipename"
          v-model="$v.form.recipename.$model"
          type="text"
          :state="validateState('recipename')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.recipename.required">
          Recipe name is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.recipename.alpha">
          Recipe name must be letters only
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="input-group-cuisine"
        label-cols-sm="3"
        label="Cuisine:"
        label-for="cuisine"
      >
        <b-form-select
          id="cuisine"
          v-model="$v.form.cuisine.$model"
          :options="cuisines"
          :state="validateState('cuisine')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Cuisine is required
        </b-form-invalid-feedback>
      </b-form-group>

       <b-form-group
        id="input-group-diet"
        label-cols-sm="3"
        label="Diet:"
        label-for="diet"
      >
        <b-form-select
          id="diet"
          v-model="$v.form.diet.$model"
          :options="diets"
          :state="validateState('diet')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Diet is required
        </b-form-invalid-feedback>
      </b-form-group>

       <b-form-group
        id="input-group-intolerance"
        label-cols-sm="3"
        label="Intolerance:"
        label-for="intolerance"
      >
        <b-form-select
          id="intolerance"
          v-model="$v.form.intolerance.$model"
          :options="intolerances"
          :state="validateState('intolerance')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Intolerance is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="input-group-resnum"
        label-cols-sm="3"
        label="Result number:"
        label-for="resnum"
      >
        <b-form-select
          id="resnum"
          v-model="$v.form.resnum.$model"
          :options="['5', '10', '15']"
          :state="validateState('resnum')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Result number is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Search</b-button
      >
    </b-form>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Search failed: {{ form.submitError }}
    </b-alert>
    <!-- <b-card class="mt-3 md-3" header="Form Data Result">
      <pre class="m-0"><strong>form:</strong> {{ form }}</pre>
      <pre class="m-0"><strong>$v.form:</strong> {{ $v.form }}</pre>
    </b-card> -->

  <div>
    <br>
    <br>
    <b-form v-if="this.show_recipes" @submit.prevent="Sort">
      <h1 class="title">Sorting options</h1>
      <b-form-group
        id="input-group-sortpick"
        label-cols-sm="3"
        label="Sort by:"
        label-for="sortpick"
      >
        <b-form-select
          id="sortpick"
          v-model="$v.form.sortpick.$model"
          :options="['Likes', 'Time to make']"
          :state="validateState('sortpick')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Sort type is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-button
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Sort</b-button
      >
    </b-form>
    <br>
    <br>
    <RecipeList v-if="this.show_recipes" :isPreview="false" :recipes="this.recipes" title="Search results"/>
  </div>
  </div>


</template>

<script>
import cuisines from "../assets/cuisines";
import diets from "../assets/diets";
import intolerances from "../assets/intolerances";
import {
  required,
  minLength,
  maxLength,
  alpha,
  sameAs
} from "vuelidate/lib/validators";
import M from "minimatch";
import RecipeList from "../components/RecipeList.vue";

export default {
    name: "Search",
    data() {
        return {
            form: {
                recipename: "",
                cuisine: "Any",
                diet: "Any",
                intolerance: "Any",
                resnum: "5",
                submitError: undefined,
                sortpick: "Likes"
            },
            cuisines: [{ value: null, text: "", disabled: true }],
            diets: [{ value: null, text: "", disabled: true }],
            intolerances: [{ value: null, text: "", disabled: true }],
            errors: [],
            validated: false,
            show_recipes: false
        };
    },
    validations: {
        form: {
            recipename: {
                required,
                length: (u) => minLength(1)(u) && maxLength(50)(u),
                alpha
            },
            cuisine: {
                required
            },
            diet: {
                required
            },
            intolerance: {
                required
            },
            resnum: {
                required,
            },
            sortpick:{
                required,
            }
        }
    },
    mounted() {
        // console.log("mounted");
        this.cuisines.push(...cuisines);
        this.diets.push(...diets);
        this.intolerances.push(...intolerances);

        this.show_recipes = false;


        if (sessionStorage.username)
        {
          console.log("sessionStorage.username PRE")
          console.log(sessionStorage.username)
          console.log("sessionStorage.search PRE")
          console.log(sessionStorage.search)
          if(sessionStorage.search)
          {
            this.recipes = JSON.parse(sessionStorage.search);
            this.show_recipes = true;
          }
        }
        // console.log($v);
    },
    methods: {
        validateState(param) {
            const { $dirty, $error } = this.$v.form[param];
            return $dirty ? !$error : null;
        },
        async Search() {
            try {
                if (this.form.cuisine == "any")
                  this.cuisine = undefined
                if (this.form.diet == "any")
                  this.diet = undefined
                if (this.form.intolerance == "any")
                  this.intolerance = undefined

                const response = await this.axios.post(
                // "https://test-for-3-2.herokuapp.com/user/Register",
                // this.$root.store.server_domain + "/Register"

                "http://localhost:3000/recipes/search", 
                {
                    query: this.form.recipename,
                    cuisine: this.cuisine,
                    diet: this.diet,
                    intolerances: this.intolerance,
                    number: this.form.resnum,
                }
                );

                console.log("response")
                console.log(response)
                console.log("response.data")
                console.log(response.data)
                this.recipes = [];
                this.recipes.push(...response.data);

                if (sessionStorage.username)
                {
                  sessionStorage.search = this.recipes;
                  console.log("sessionStorage.search POST")
                  console.log(sessionStorage.search = JSON.stringify(this.recipes))
                }
                //console.log(this.recipes);


                this.show_recipes = true;
            }
            catch (err) {
                console.log(err.response);
                this.form.submitError = err.response.data.message;
            }
        },

        Sort() {
                if (this.form.sortpick == "Likes")
                {
                  this.recipes = this.recipes.sort((recipe1,recipe2) => recipe1.aggregateLikes > recipe2.aggregateLikes ? -1 : 1);
                  console.log(this.recipes);
                }

                if (this.form.sortpick == "Time to make")
                {
                  this.recipes = this.recipes.sort((recipe1,recipe2) => recipe1.readyInMinutes > recipe2.readyInMinutes ? -1 : 1);
                  console.log(this.recipes);
                }

                if (sessionStorage.username)
                {
                  sessionStorage.search = JSON.stringify(this.recipes);
                }

                this.show_recipes = false;

                 setTimeout(() => {
                  this.show_recipes = true;
                }, 10);
        },

        onSearch() {
            // console.log("search method called");
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            // console.log("serach method go");
            this.Search();
        },
        onReset() {
            this.form = {
                recipename: "",
                cuisine: "Any",
                diet: "Any",
                intolerance: "Any",
                resnum: "5",
                sortpick: "Likes"
            };
            this.$nextTick(() => {
                this.$v.$reset();
            });
        }
    },
    components: { RecipeList }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>
