<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="lift in lifts">
      <p> ID: {{ lift.id }} </p>
      <p> Exercise id: {{ lift.exercise_id }} </p>
      <p> Reps: {{ lift.reps }} </p>
      <p> Weight: {{ lift.weight }} lbs</p>
      <p> Sets: {{ lift.sets}} </p>
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
        message: "Workout",
        lifts: []
      };
    },
    created: function () {
      this.liftIndex();
    },
    methods: {
      liftIndex: function() {
       axios.get("/lifts").then((response) => {console.log("lift index", response); 
        this.lifts = response.data;
      });
      },
    },
  };
</script>
