<template>
<div class="recipe-box">
  <div class="recipe-preview">
  <router-link
    :to="{ path: '/recipe/' + recipe.id
    //, params: { recipeId: recipe.id } 
    }"
  >
    <div class="recipe-body">
      <img v-if="image_load" :src="recipe.image" class="recipe-image" />
    </div>
    <div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title">
        <h4>{{ recipe.title }}</h4>
      </div>
    </div>
  </router-link>
  
    <div >
      <b-row class="recipe-overview">
        <b-col><h5>Time to make:</h5> {{ recipe.readyInMinutes }} minutes</b-col>
        <b-col><h5>Popularity:</h5> {{ recipe.aggregateLikes }} likes</b-col>
      </b-row>
    </div>
    
    <div>
      <b-col >
        <br>
        <h5>Instructions:</h5>
        <b-row v-for="instruction in recipe.instructions[0]" :key="instruction">
          <li>{{instruction}}</li>
          
        </b-row>
      </b-col>
    </div>
</div>
</div>
</template>

<script>
export default {
  mounted() {
    this.axios.get(this.recipe.image).then((i) => {
      this.image_load = true;
    });
  },
  data() {
    return {
      image_load: false
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    }

    // id: {
    //   type: Number,
    //   required: true
    // },
    // title: {
    //   type: String,
    //   required: true
    // },
    // readyInMinutes: {
    //   type: Number,
    //   required: true
    // },
    // image: {
    //   type: String,
    //   required: true
    // },
    // aggregateLikes: {
    //   type: Number,
    //   required: false,
    //   default() {
    //     return undefined;
    //   }
    // }
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
.recipe-preview > .recipe-body {
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

.recipe-title 
{
padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
  text-decoration: underline;
}

h4, h5
{
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

recipe-overview
{
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
  padding: var(--b);
  border: calc(2*var(--b)) solid #0000;
  outline: 1px solid #000;
  outline-offset: calc(-1*var(--b));
  background: conic-gradient(from 90deg at 1px 1px,#0000 90deg,#000 0);
}

li {
    margin-top: 15px;
}



</style>
