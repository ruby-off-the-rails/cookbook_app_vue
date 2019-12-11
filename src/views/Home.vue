<template>
  <div class="home">
    <p>Add the title of your new recipe here<input type="text" v-model="newRecipeTitle"></p>
    <p>Add the Chef of your new recipe here<input type="text" v-model="newRecipeChef"></p>
    <p>Add the Ingredients of your new recipe here<input type="text" v-model="newRecipeIngredients"></p>
    <p>Add the PrepTime of your new recipe here<input type="text" v-model="newRecipePrepTime"></p>
    <p>Add the Directions of your new recipe here<input type="text" v-model="newRecipeDirections"></p>
    <button v-on:click="addRecipe()">Add a new recipe</button>
    <div v-for="recipe in recipes">
     <p>title: {{ recipe.title }}</p>
     <p>ingredients: {{ recipe.ingredients }}</p>
     <img v-bind:src="recipe.image_url" v-bind:alt="recipe.title">
     <br>

     <button v-on:click="toggleInfo(recipe)">See more info</button>

     <div v-if="currentRecipe === recipe">
       <p>prep_time: {{ recipe.prep_time }}</p>
       <p>directions: {{ recipe.directions }}</p>
     </div>
     <hr>
    </div>


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
      recipes: [],
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipePrepTime: "",
      newRecipeDirections: "",
      currentRecipe: {}
    };
  },
  created: function() {
    console.log('i am in created');
    axios.get("/api/recipes").then(response => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {
    addRecipe: function() {
      console.log('adding the receip...');

      var params = {
        chef: this.newRecipeChef,
        title: this.newRecipeTitle,
        ingredients: this.newRecipeIngredients,
        directions: this.newRecipeDirections,
        prep_time: this.newRecipePrepTime
      };


      axios.post("/api/recipes", params).then(response => {
        console.log(response.data);
        this.recipes.push(response.data);
        this.newRecipeTitle = "";
        this.newRecipeChef = "";
        this.newRecipeIngredients = "";
        this.newRecipePrepTime = "";
        this.newRecipeDirections = "";
      });
    },
    toggleInfo: function(theRecipe) {
      console.log(theRecipe);
      this.currentRecipe = theRecipe;
      console.log('in toggle info...');
    }
  }
};
</script>
