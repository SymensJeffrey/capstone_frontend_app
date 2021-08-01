<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <hr />
    <div v-for="exercise in exercises">
      ID:<p>{{ exercise.id }}</p>
      Name:<p>{{ exercise.name }}</p>
      Description<p>{{ exercise.description }}</p>
      <button v-on:click="exerciseShow()"> More Info </button>
      <hr />
    </div>
  </div>
</template>

<style></style>

<script>
  import axios from "axios"
  export default {
    data: function () {
      return {
        message: "GYMOLOGY",
        exercises: [],
      };
    },
    created: function () {
      this.exercisesIndex()
    },
    methods: {
      exercisesIndex: function() {
        console.log("indexing...")
        axios.get("/exercises").then((response) => {console.log("exercises index", response); 
        this.exercises = response.data;
      });
      },
      exerciseShow: function() {
        console.log("showing...")
      }
    },
  };
</script>
