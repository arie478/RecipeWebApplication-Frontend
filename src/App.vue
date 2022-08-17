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
            <b-dropdown-item :to="{ name: 'createRecipe' }">Create Personal Recipe</b-dropdown-item>
            <b-dropdown-divider></b-dropdown-divider>
            <b-dropdown-item @click="Logout">Log Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",

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
    }
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
