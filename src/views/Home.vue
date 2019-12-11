<template>
  <div class="home">
    <div v-for="recipe in recipes">
     <p>title: {{ recipe.title }}</p>
     <p>ingredients: {{ recipe.ingredients }}</p>
     <p>{{ recipe.image_url }}</p>
     <img v-bind:src="recipe.image_url" v-bind:alt="recipe.title">
     <br>

     <hr>
    </div>

    <h1>{{ message }}</h1>
    <h1>{{ name }}</h1>
    <h1>{{ recipes }}</h1>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
// var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Welcome to actualize!",
      name: "Brian",
      recipes: []
    };
  },
  created: function() {
    console.log('i am in created');
    axios.get("/api/recipes").then(response => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {}
};
</script>
