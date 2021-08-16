<template>
  <div class="home">
    <div class="container"
    <h1>{{ message }}</h1>
    <br>
    <br>
    <div class="workout-text-center">
    <button v-if="!isLiftsEmpty()" v-on:click="workoutCreate()" class="btn btn-outline-secondary button-margin">Finish Workout</button>
    <button v-if="!isLiftsEmpty()" v-on:click="pushToHome()" class="btn btn-outline-secondary btn-text-center">Add Another Exercise</button>
    </div>
    <br>
    <br>
    <div class="workout-text-center">
      <h2>{{noLiftMessage}}</h2>
    </div>
    <div class="container">
      <div v-for="lift in lifts">
        <div class="card">
          <h5 class="card-header">{{ lift.exercise.name }}</h5>
          <div class="card-body">
            <h5 class="card-title">Reps: {{ lift.reps }} | Weight: {{ lift.weight }}lbs | Sets: {{ lift.sets}}
            <button class="btn btn-outline-secondary round-button align-right">X</button>
            </h5>
          </div>
        </div>
      </div>
    </div>
    <br>
  </div>
</template>

<style></style>

<script>
  import axios from "axios"
  export default {
    data: function () {
      return {
        message: "Workout",
        lifts: [],
        noLiftMessage: ""
      };
    },
    created: function () {
      this.liftIndex();
      this.isLiftsEmpty();
    },
    methods: {
      liftIndex: function() {
       axios.get("/lifts").then((response) => {console.log("lift index", response); 
        this.lifts = response.data;
      });
      },
      workoutCreate: function() {
        console.log("creating workout...")
        axios.post("/workouts").then((response) => {console.log("workout create", response);
          this.workout = response.data;
        });
        this.$router.push("/profile");
      },
      pushToHome: function() {
        this.$router.push("/");
      },
      isLiftsEmpty: function() {
        var lifts = this.lifts
        if(lifts.length == 0){
          this.noLiftMessage = "You currently dont have any exercises added to your workout"
          return true
        } else {
          this.noLiftMessage = ""
          return false
        }
      }
    },
  };
</script>
