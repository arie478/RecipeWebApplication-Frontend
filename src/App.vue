<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <router-link :to="{ name: 'about' }">About</router-link>|
        <!-- <router-link :to="{ path: '/recipe/641958' }">Recipe 641958</router-link>| -->
      <!-- {{ !$root.store.username }} -->
      <span v-if="!$root.store.username">
        Hello Guest
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        <div>
          <b-dropdown id="dropdown-divider" :text='$root.store.username + "`s profile"' class="m-2">
            <b-dropdown-item :to="{name: 'favorites'}"> My Favorites </b-dropdown-item>
            <b-dropdown-item :to="{name: 'myRecipes'}"> My Recipes </b-dropdown-item>
            <b-dropdown-item :to="{name: 'familyRecipes'}"> Family Recipes </b-dropdown-item>
            <b-dropdown-item :to="{name: 'createRecipe'}"> Create New Recipe </b-dropdown-item>
            <b-dropdown-divider></b-dropdown-divider>
            <b-dropdown-item-button @click="Logout"> Logout </b-dropdown-item-button>
          </b-dropdown>
        </div>
        <!-- {{ $root.store.username }}
        <router-link :to="{ name: 'main' }"> My Favorites</router-link>|
        <router-link :to="{ name: 'main' }"> My Recipes</router-link>|
        <router-link :to="{ name: 'main' }"> Family Recipes</router-link>|
        <router-link :to="{ name: 'main' }"> Create New Recipe</router-link>|
        <button @click="Logout">Logout</button>| -->
      </span>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
