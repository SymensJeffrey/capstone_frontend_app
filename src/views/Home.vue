<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      <p>{{ exercises }}</p>
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
    },
  };
</script>
