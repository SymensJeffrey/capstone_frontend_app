<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <br>
    <br>
    <div class="workout-text-center">
    <button v-if="!isLiftsEmpty()" v-on:click="workoutCreate()" class="btn btn-outline-secondary">Finish Workout</button>
    <h2>{{noLiftMessage}}</h2>
    <div class="row align-items-center">
      <div class="col-12" v-for="lift in lifts">
        <p>
          Name: {{ lift.exercise.name }} |
          Reps: {{ lift.reps }} |
          Weight: {{ lift.weight }} lbs |
          Sets: {{ lift.sets}}
        </p>
        <hr />
      </div>
    </div>
    <button v-if="!isLiftsEmpty()" v-on:click="pushToHome()" class="btn btn-outline-secondary btn-text-center">Add Another Exercise</button>
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
