<template>
  <div class="home">
    <br>
    <br>
    <br>
    <br>
    <h1 class="vertical-center">Hello {{ user.name }}</h1>
    <div class="container">
      <div v-for=" workout in workouts">
        <p>Date: {{ workout.date }} </p>
        <div v-for="lift in workout.lifts">
          Reps: {{lift.reps}}
          Weight: {{lift.weight}}
          Sets: {{lift.sets}}
        </div>
        <div v-for="exercise in workout.exercises">
          {{exercise.name}}
        </div>
        <hr>
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
      // workoutShow: function(workout) {
      //   this.currentWorkout = workout
      //   axios.get(`/workouts/${workout.id}`).then((response) => {console.log("workoutshow", response);
      //   this.currentWorkout = response.data;
      //   this.lifts = this.currentWorkout.lifts
      //   this.exercises = this.currentWorkout.exercises
      //   })
      //   document.querySelector("#workout-details").showModal();
      // },
    },
  };
</script>
