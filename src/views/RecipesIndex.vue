<template>
  <div class="recipes-index">
    <h1>{{ message }}</h1>

    <div v-for="recipe in recipes">
      <p>{{ recipe.title }}</p>
      <p>{{ recipe.chef }}</p>
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to recipes index!",
      recipes: []
    };
  },
  created: function() {
    axios.get("/api/recipes").then(response => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {}
};
</script>
