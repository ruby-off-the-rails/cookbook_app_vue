<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>{{ recipe.id }}</h1>
    <h1>{{ recipe.title }}</h1>
    <h1>{{ recipe.chef }}</h1>
    <h1>{{ recipe.ingredients }}</h1>
    <h1>{{ recipe.directions }}</h1>
    <h1>{{ recipe.prep_time }}</h1>
    <h1>{{ recipe.image_url }}</h1>
    <img v-bind:src="recipe.image_url">
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to the show",
      recipe: {}
    };
  },
  created: function() {
    console.log(this.$route.params.id);
    axios.get(`/api/recipes/${this.$route.params.id}`).then(response => {
      // console.log(response.data);
      this.recipe = response.data;
    });
  },
  methods: {}
};
</script>
