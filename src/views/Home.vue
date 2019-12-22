<template>
  <div class="home">
    <p>Add the title of your new recipe here<input type="text" v-model="newRecipeTitle"></p>
    <p>Add the Chef of your new recipe here<input type="text" v-model="newRecipeChef"></p>
    <p>Add the Ingredients of your new recipe here<input type="text" v-model="newRecipeIngredients"></p>
    <p>Add the PrepTime of your new recipe here<input type="text" v-model="newRecipePrepTime"></p>
    <p>Add the Directions of your new recipe here<input type="text" v-model="newRecipeDirections"></p>
    <button v-on:click="addRecipe(recipe)">Add a new recipe</button>
    <br>
    <br>
    Search for products here:<input type="text" v-model="searchTerm" list="titles">
    <datalist id="titles">
      <option v-for="recipe in recipes">{{ recipe.title }}</option>
    </datalist>
    <br>
    <br>
    <button v-on:click="setSortAttribute('prep_time')">Sort by prep_time</button>
    <button v-on:click="setSortAttribute('title')">Sort by title</button>
    <hr>
    <!-- <div v-for="recipe in filterBy(recipes, searchTerm, 'title', 'ingredients')" v-bind:class="{selected: recipe.selected}"> -->

    <transition-group appear enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
      <div v-for="recipe in orderBy(filterBy(recipes, searchTerm, 'title', 'ingredients'), sortAttribute, sortOrder)" v-bind:class="{selected: recipe.selected}" v-bind:key="recipe.id">
        <button v-on:click="selectRecipe(recipe)">Select Recipe</button>
        <!-- Recipe.where(title: "apple") -->
        <p>id: {{ recipe.id }}</p>
        <p>title: {{ recipe.title }}</p>
        <p>prep_time: {{ recipe.friendly_prep_time }}</p>
        <p>ingredients: {{ recipe.ingredients }}</p>
        <img v-bind:src="recipe.image_url" v-bind:alt="recipe.title">
        <br>

        <button v-on:click="toggleInfo(recipe)">See more info</button>

        <div v-if="currentRecipe === recipe">
          <p>prep_time: {{ recipe.prep_time }}</p>
          <p>directions: {{ recipe.directions }}</p>

          <p>Title: <input type="text" v-model="recipe.title"></p>
          <p>Ingredients: <input type="text" v-model="recipe.ingredients"></p>
          <p>Directions: <input type="text" v-model="recipe.directions"></p>
          <p>Prep Time: <input type="text" v-model="recipe.prep_time"></p>
          <p>Chef: <input type="text" v-model="recipe.chef"></p>
          <button v-on:click="updateRecipe(recipe)">Update the recipe</button>
          <button v-on:click="destroyRecipe(recipe)">Destroy Recipe</button>
        </div>
        <hr>
      </div>
    </transition-group>


  </div>
</template>

<style>
.wild {
  background-color: red;
}


.selected {
  color: white;
  background-color: steelBlue;
  transition: background-color 1s ease;
}
</style>

<script>
import axios from "axios";
// var axios = require("axios");
import Vue2Filters from 'vue2-filters'

import "animate.css/animate.css"


export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to actualize!",
      searchTerm: '',
      name: "Brian",
      recipes: [],
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipePrepTime: "",
      newRecipeDirections: "",
      currentRecipe: {},
      sortAttribute: 'title',
      sortOrder: 1
    };
  },
  created: function() {
    console.log(axios.defaults.headers.common["Authorization"]);
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
      if (this.currentRecipe === theRecipe) {
        this.currentRecipe = null;
      } else {
        this.currentRecipe = theRecipe;
      }
      console.log('in toggle info...');
    },
    updateRecipe: function(theRecipe) {
      console.log('updating the recipe...');
      console.log(theRecipe);

      var params = {
        title: theRecipe.title,
        chef: theRecipe.chef,
        ingredients: theRecipe.ingredients,
        directions: theRecipe.directions,
        prep_time: theRecipe.prep_time,
      }

      axios.patch(`/api/recipes/${theRecipe.id}`, params).then(response => {
        console.log(response.data);
        theRecipe.title = response.data.title;
        theRecipe.directions = response.data.directions;
        theRecipe.ingredients = response.data.ingredients;
        theRecipe.prep_time = response.data.prep_time;
        theRecipe.chef = response.data.chef;
      });
    },
    destroyRecipe: function(theRecipe) {
      // tell rails to delete the recipe
      console.log(theRecipe);
        // remove the recipe from the screen
        // find the index in the array
        // remove that index from the array


      axios.delete(`/api/recipes/${theRecipe.id}`).then(response => {
        console.log(response.data);
        var index = this.recipes.indexOf(theRecipe);
        console.log(index);
        this.recipes.splice(index, 1);

      });
    },
    selectRecipe: function(theRecipe) {
      console.log('selecting the recipe');
      // if (theRecipe.selected === true) {
      //   theRecipe.selected = false;
      // } else {
      //   theRecipe.selected = true;
      // }
      theRecipe.selected = !theRecipe.selected;
      console.log(theRecipe);
    },
    setSortAttribute: function(theAttribute) {
      this.sortAttribute = theAttribute;
      // this.sortOrder = -1;
      console.log(this.sortOrder === 1);
      if (this.sortOrder === 1) {
        this.sortOrder = -1;
      } else {
        this.sortOrder = 1;
      }
    }
  }
};
</script>
