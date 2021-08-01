<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <hr />
    <div v-for="exercise in exercises">
      <p>ID:{{ exercise.id }}</p>
      <p>Name:{{ exercise.name }}</p>
      <button v-on:click="exerciseShow(exercise)"> More Info </button>
      <button v-on:click="addToWorkoutPopup(exercise)">Add to workout</button> 
      <hr />
    </div>
    <dialog id="exercise-details">
      <form method="dialog">
        <h2>{{ currentExercise.name }}</h2>
        <p>ID:{{ currentExercise.id }}</p>
        <p>Name:{{ currentExercise.name }}</p>
        <p>Description:{{ currentExercise.description }}</p>
        <p>Equipment:{{ currentExercise.equipment }}</p>
        <p>Muscles:{{ currentExercise.category }}</p>
        <button>Close</button>
      </form>
    </dialog>
    <dialog id="lift-create">
      <form method="dialog">
        <h2>Adding {{ currentExercise.name }}</h2>
        <p> Reps: <input type="number" v-model="newLiftParams.reps"> </p>
        <p> Weight: <input type="number" v-model="newLiftParams.weight"> </p>
        <p> Sets: <input type="number" v-model="newLiftParams.sets"> </p>
        <button>Close</button>
        <button v-on:click="liftCreate()">Add to Workout</button>
      </form>
    </dialog>
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
        currentExercise: {},
        newLiftParams: {}
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
      exerciseShow: function(exercise) {
        console.log("showing...")
        this.currentExercise = exercise;
        document.querySelector("#exercise-details").showModal();
      },

      addToWorkoutPopup: function(exercise) {
        this.currentExercise = exercise;
        document.querySelector("#lift-create").showModal();
      },

      liftCreate: function() {
        console.log("creating lift...")
      },
    },
  };
</script>