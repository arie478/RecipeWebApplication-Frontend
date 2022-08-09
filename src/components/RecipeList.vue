<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-col v-if="this.isPreview">
      <b-row v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
    <b-col v-if="!this.isPreview">
      <b-row v-for="r in recipes" :key="r.id">
        <RecipeFull class="recipeFull" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
import RecipeFull from "./RecipeFull.vue";
export default {
  name: "RecipeList",
  components: {
    RecipePreview,
    RecipeFull
  },
  props: {
    title: {
      type: String,
      required: true
    },
     recipes: 
    {
      type: Array,
      required: true
    },
     isPreview: 
    {
      type: Boolean,
      required: true
    }
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
