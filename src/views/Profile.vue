<template>
  <div class="home">
    <h1>Hello </h1>
    <div v-for=" workout in workouts">
      <p>ID: {{ workout.id }}</p>
      <p>User ID: {{ workout.user_id }}</p>
      <p>Date: {{ workout.date }}</p>

    </div>
    </div>
  </div>
</template>

<style></style>

<script>
  import axios from "axios"
  export default {
    data: function () {
      return {
        workouts: []
      };
    },
    created: function () {
      this.workoutIndex()
    },
    methods: {
      workoutIndex: function() {
        console.log("workout index...")
        axios.get("/workouts").then((response) => {console.log("workout index", response); 
        this.workouts = response.data;
      });
      },
    },
  };
</script>
