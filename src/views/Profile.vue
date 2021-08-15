<template>
  <div class="home">
    <br>
    <br>
    <br>
    <br>
    <h1>Hello {{ user.name }}</h1>
    <div v-for=" workout in workouts">
      <p>Date: {{ workout.date }}</p>
      <p><button v-on:click="workoutShow(workout)" class="btn btn-outline-secondary round-button">View Workout</button></p>
    </div>
    <dialog id="workout-details">
      <form method="dialog" class="lift-create">
        <p> {{currentWorkout.date}} </p>
        <p> {{currentWorkout.lift}} </p>
        <p> {{currentWorkout.exercise}}</p>
        <div class="container">
          <div class="row">
          <div class="col-lg-6">
            <div class="word-wrap" v-for="exercise in exercises">
              <div>{{exercise.name}}</div>
            </div>
          </div>
          <div class="col-lg-6">
            <div v-for="lift in lifts">
              <div class="word-wrap form-right-margin">
              Weight:{{lift.weight}} lbs |
              Reps:{{lift.reps}} |
              Sets:{{lift.sets}} </div>
            </div>
          </div>
          </div>
            <button class="btn btn-outline-secondary round-button">Close</button>
        </div>
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
        workouts: [],
        user: "",
        currentWorkout: {},
        lifts: [],
        exercises: [],
      };
    },
    created: function () {
      this.workoutIndex();
      this.userShow();
    },
    methods: {
      workoutIndex: function() {
        axios.get("/workouts").then((response) => {console.log("workout index", response); 
        this.workouts = response.data;
      });
      },
      userShow: function() {
        axios.get(`/users/${localStorage.user_id}`).then((response) => {console.log(response)
        this.user = response.data;
        });
      },
      workoutShow: function(workout) {
        this.currentWorkout = workout
        axios.get(`/workouts/${workout.id}`).then((response) => {console.log("workoutshow", response);
        this.currentWorkout = response.data;
        this.lifts = this.currentWorkout.lifts
        this.exercises = this.currentWorkout.exercises
        })
        document.querySelector("#workout-details").showModal();
      },
    },
  };
</script>
