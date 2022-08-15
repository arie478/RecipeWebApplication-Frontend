<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <RecipeViewer class="recipeFull" :recipe="r" :show_ing_and_serv="show_ing_and_serv" :isPreview="isPreview" />
      </b-row>
    </b-col>
  </b-container>
</template>

<script>
import RecipeViewer from "./RecipeViewer.vue";
export default {
  name: "RecipeList",
  components: {
    RecipeViewer
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
    },
    show_ing_and_serv:
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
