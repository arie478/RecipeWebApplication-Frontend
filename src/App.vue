<template>
  <div id="app">

    <!-- <b-navbar toggleable="xl" variant="primary"> -->
    <!-- <b-navbar toggleable="xl" variant="success"> -->
    <b-navbar toggleable="xl" variant="info">
    <!-- <b-navbar toggleable="xl" variant="warning"> -->
    <!-- <b-navbar toggleable="xl" variant="danger"> -->
    <!-- <b-navbar toggleable="xl" variant="dark"> -->

      <b-navbar-brand :to="{ name: 'main' }">FoodIsGood</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item :to="{ name: 'main' }">Home</b-nav-item>
          <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
          <b-nav-item :to="{ name: 'about' }">About</b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto" v-if="!$root.store.username">
          <b-nav-item>Hello Guest!</b-nav-item>
          <b-nav-item :to="{ name: 'login' }">Login</b-nav-item>
          <b-nav-item :to="{ name: 'register' }">Register</b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto" v-else>
          <b-nav-item-dropdown right>
            <template #button-content v-if="$root.store.username">{{ $root.store.username }}'s profile</template>

            <!-- <h1 class="title" v-if="$root.store.username">WELCOME {{ $root.store.username }}</h1> -->
            <template v-else #button-content> User </template>
            <b-dropdown-item :to="{ name: 'favorites' }">Favorite Recipes</b-dropdown-item>
            <b-dropdown-item :to="{ name: 'myRecipes' }">Personal Recipes</b-dropdown-item>
            <b-dropdown-item :to="{ name: 'familyRecipes' }">Family Recipes</b-dropdown-item>
            <b-dropdown-item v-b-modal.modal-new-personal-recipe>Create Personal Recipe</b-dropdown-item>
            <b-dropdown-divider></b-dropdown-divider>
            <b-dropdown-item @click="Logout">Log Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>


    <b-modal
    :hide-footer="true"
    id="modal-new-personal-recipe"
    scrollable
    title="Create Personal Recipe"
    >
      <b-form @submit.prevent="onCreate" @reset.prevent="onReset">
      <b-form-group
        id="input-group-tilte"
        label-cols-sm="3"
        label="Title:"
        label-for="title"
      >
        <b-form-input
          id="title"
          v-model="$v.form.title.$model"
          type="text"
          :state="validateState('title')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.title.required">
          Recipe needs to have a title
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.title.length">
          Recipe title should be at least 1 letter long
        </b-form-invalid-feedback>
      </b-form-group>


      <b-form-group
        id="input-group-readyInMinutes"
        label-cols-sm="3"
        label="Minutes to make:"
        label-for="readyInMinutes"
      >
        <b-form-input
          id="readyInMinutes"
          v-model="$v.form.readyInMinutes.$model"
          type="text"
          :state="validateState('readyInMinutes')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">
          Need to specify how long to make
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.numeric">
          Time to make should be a number
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="input-group-image"
        label-cols-sm="3"
        label="Image URL:"
        label-for="image"
      >
        <b-form-input
          id="image"
          v-model="$v.form.image.$model"
          type="text"
          :state="validateState('image')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.image.required">
          Image URL is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.image.url">
          Image URL should be a valid URL
        </b-form-invalid-feedback>
      </b-form-group>
      

      <b-form-group
        id="input-group-vegan"
        label-cols-sm="3"
        label="Vegan:"
        label-for="vegan"
      >
        <b-form-select
          id="vegan"
          v-model="$v.form.vegan.$model"
          :options="['No', 'Yes']"
          :state="validateState('vegan')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Vegan option is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="input-group-vegetarian"
        label-cols-sm="3"
        label="Vegetarian:"
        label-for="vegetarian"
      >
        <b-form-select
          id="vegetarian"
          v-model="$v.form.vegetarian.$model"
          :options="['No', 'Yes']"
          :state="validateState('vegetarian')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Vegetarian option is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="input-group-glutenFree"
        label-cols-sm="3"
        label="Gluten Free:"
        label-for="glutenFree"
      >
        <b-form-select
          id="glutenFree"
          v-model="$v.form.glutenFree.$model"
          :options="['No', 'Yes']"
          :state="validateState('glutenFree')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Gluten Free option is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="input-group-ingredients"
        label-cols-sm="3"
        label="Ingredients to make:"
        label-for="ingredients"
      >
       <b-form-textarea
        id="ingredients"
        v-model="ingredients"
        placeholder="Write one ingredient per line, for example: 1 large onion"
        rows="3"
        max-rows="8"
      ></b-form-textarea>
       </b-form-group>

       <b-form-group
        id="input-group-instructions"
        label-cols-sm="3"
        label="Instructions how to make:"
        label-for="instructions"
      >
       <b-form-textarea
        id="instructions"
        v-model="instructions"
        placeholder="Write one instructions per line, for example: Dice the onion, put on the side for later"
        rows="3"
        max-rows="8"
      ></b-form-textarea>
       </b-form-group>

      <b-form-group
        id="input-group-servings"
        label-cols-sm="3"
        label="How many servings:"
        label-for="servings"
      >
        <b-form-input
          id="servings"
          v-model="$v.form.servings.$model"
          type="text"
          :state="validateState('servings')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.servings.required">
          Need to specify how many servings
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.servings.numeric">
          Servings count should be a number
        </b-form-invalid-feedback>
      </b-form-group>
    

      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Create</b-button
      >
    </b-form>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Register failed: {{ form.submitError }}
    </b-alert>
    </b-modal>



    <router-view />
  </div>
</template>

<script>

import {
  required,
  minLength,
  maxLength,
  alpha,
  sameAs,
  email,
  numeric,
  url
} from "vuelidate/lib/validators";

export default {
  name: "App",

  data() 
  {
    return {
      form: 
      {
        title: "",
        readyInMinutes: "",
        image: "",
        vegan: "No",
        vegetarian: "No",
        glutenFree: "No",
        servings: "",

        submitError: undefined
      },
      errors: [],
      validated: false,
      ingredients: "",
      instructions: ""
    };
  },

  validations: 
  {
    form: 
    {
      title: 
      {
        required,
        length: (u) => minLength(1)(u),
      },
      readyInMinutes: { required, numeric },
      image: { required, url },
      vegan: { required },
      vegetarian: { required },
      glutenFree: { required },
      servings: {required, numeric},
      
    },

    ingredients: {required},
    instructions: {required}
  },

  methods:
  {
    async Logout() {
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.post(
          "http://localhost:3000/Logout"
        );
        this.axios.defaults.withCredentials = false;
        console.log(response);
      }
      catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
        return;
      }
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");
      this.$router.push("/").catch(() => { this.$forceUpdate(); });
    },

     validateState(param) 
    {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },

    async CreateRecipe() 
    {
      try 
      {
        

        let ingredientsList = this.ingredients.split('\n');
        let ingredientsJson = JSON.parse(JSON.stringify(ingredientsList));

        let  instructionsList =  this.instructions.split('\n');
        let instructionsJson = JSON.parse(JSON.stringify(instructionsList));


         this.axios.defaults.withCredentials = true;
        const response = await this.axios.post(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          // this.$root.store.server_domain + "/Register",
          "http://localhost:3000/users/createPersonalRecipe",
          { 
            title: this.form.title,
            readyInMinutes: this.form.readyInMinutes,
            image: this.form.image,
            vegan: this.form.vegan,
            vegetarian: this.form.vegetarian,
            glutenFree: this.form.glutenFree,
            ingredients: ingredientsJson,
            instructions: instructionsJson,
            servings : this.form.servings
          }
        );
         this.axios.defaults.withCredentials = false;
         
        console.log(response);
      }
      catch (err) 
      {
        console.log(err);
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },



     onCreate() 
    {
      console.log("create method called");
      this.$v.form.$touch();
      if (this.$v.form.$anyError) { return; }
      console.log("create method go");
      this.CreateRecipe();
    },

    onReset() 
    {
      this.form = 
      {
        title: "",
        readyInMinutes: "",
        image: "",
        vegan: "No",
        vegetarian: "No",
        glutenFree: "No",
        servings: "",

        
      };
      this.ingredients= "",
      this.instructions= ""

      this.$nextTick(() => { this.$v.$reset(); });
    },

  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";



#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #000000;
}

#nav a.router-link-exact-active {
  color: #000000;
}
</style>
