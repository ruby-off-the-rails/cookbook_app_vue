<template>
  <div class="recipes-new">
    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>Make a new recipe</h1>
        <div class="form-group">
          <label>Title:</label>
          <input type="text" class="form-control" v-model="title">
        </div>
        <div class="form-group">
          <label>Ingredients:</label>
          <input type="text" class="form-control" v-model="ingredients">
        </div>
        <div class="form-group">
          <label>Directions:</label>
          <input type="text" class="form-control" v-model="directions">
        </div>
        <div class="form-group">
          <label>Prep Time:</label>
          <input type="text" class="form-control" v-model="prepTime">
        </div>
        <div class="form-group">
          <label>Image Url:</label>
          <input type="text" class="form-control" v-model="imageUrl">
        </div>
        <div class="form-group">
          <label>Chef:</label>
          <input type="text" class="form-control" v-model="chef">
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
      title: "orange",
      ingredients: "seeds + sun",
      directions: "time",
      prepTime: "20",
      imageUrl: "",
      chef: "lorstrom"
    };
  },
  methods: {
    submit: function() {
      var params = {
        title: this.title,
        ingredients: this.ingredients,
        directions: this.directions,
        prep_time: this.prepTime,
        image_url: this.imageUrl,
        chef: this.chef
      };
      axios
        .post("/api/recipes", params)
        .then(response => {
          this.$router.push("/recipes");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
