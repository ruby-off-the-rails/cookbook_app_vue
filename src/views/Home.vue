<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
</template>

<style>
</style>

<script>
export default {
  data: function() {
    return {
      message: "Welcome to actualize!"
    };
  },
  created: function() {
    console.log('i am in created');
  },
  methods: {}
};
</script>
