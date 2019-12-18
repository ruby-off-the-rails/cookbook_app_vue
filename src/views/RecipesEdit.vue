<template>
  <div class="recipes-edit">
    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>Edit the recipe</h1>
        {{ recipe }}
        <div class="form-group">
          <label>Title:</label>
          <input type="text" class="form-control" v-model="recipe.title">
        </div>
        <div class="form-group">
          <label>Ingredients:</label>
          <input type="text" class="form-control" v-model="recipe.ingredients">
        </div>
        <div class="form-group">
          <label>Directions:</label>
          <input type="text" class="form-control" v-model="recipe.directions">
        </div>
        <div class="form-group">
          <label>Prep Time:</label>
          <input type="text" class="form-control" v-model="recipe.prep_time">
        </div>
        <div class="form-group">
          <label>Image Url:</label>
          <input type="text" class="form-control" v-model="recipe.image_url">
        </div>
        <div class="form-group">
          <label>Chef:</label>
          <input type="text" class="form-control" v-model="recipe.chef">
        </div>
        <input type="submit" class="btn btn-primary" value="Submit">
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {}
    };
  },
  created: function() {
    console.log('in created');
    axios.get(`/api/recipes/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.recipe = response.data;
    });
  },
  methods: {
    submit: function() {
      var params = {
        title: this.recipe.title,
        ingredients: this.recipe.ingredients,
        directions: this.recipe.directions,
        prep_time: this.recipe.prep_time,
        image_url: this.recipe.image_url,
        chef: this.recipe.chef
      };
      axios
        .patch(`/api/recipes/${this.$route.params.id}`, params)
        .then(response => {
          this.$router.push(`/recipes/${this.$route.params.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
