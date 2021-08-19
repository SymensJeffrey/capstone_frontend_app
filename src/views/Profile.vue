<template>
  <div class="home black-background">
    <div class="container white-background">
      <br>
      <br>
      <br>
      <br>
      <h1 class="vertical-center">Hello {{ user.name }}</h1>
      <br>
      <br>
      <div class="container white-background">
        <div v-for=" workout in workouts">
          <div class="card border-secondary mb-3">
          <h5 class="card-header">{{ workout.date }}</h5>
            <div class="card-body">
              <div class="row">
                <div class="col exercise-names">
                  <div v-for="exercise in workout.exercises">
                    {{exercise.name}} 
                  </div>
                </div>
                <div class="col lifts-params">
                  <div v-for="lift in workout.lifts">
                    Weight: {{lift.weight}} |
                    Reps: {{lift.reps}} | 
                    Sets: {{lift.sets}} |
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
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
