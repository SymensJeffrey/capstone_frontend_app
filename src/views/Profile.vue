<template>
  <div class="home">
    <h1>Hello {{ user.name }}</h1>
    <div v-for=" workout in workouts">
      <p>ID: {{ workout.id }}</p>
      <p>User ID: {{ workout.user_id }}</p>
      <p>Date: {{ workout.date }}</p>
      <p><button v-on:click="workoutShow(workout)">View Workout</button></p>
    </div>
    <dialog id="workout-details">
      <form method="dialog">
        <p> {{currentWorkout.id}} </p>
        <p> {{currentWorkout.date}} </p>
        <p> {{currentWorkout.lift}}
        <div v-for="lift in lifts">
          <p>Name:{{lift.exercise_id}} |
          Weight:{{lift.weight}} lbs |
          Reps:{{lift.reps}} |
          Sets{{lift.sets}} </p>
        </div>
        <button>Close</button>
      </form>
    </dialog>
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
        lifts: []
      };
    },
    created: function () {
      this.workoutIndex(),
      this.userShow()
    },
    methods: {
      workoutIndex: function() {
        console.log("workout index...")
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
        })
        document.querySelector("#workout-details").showModal();
      }
    },
  };
</script>
