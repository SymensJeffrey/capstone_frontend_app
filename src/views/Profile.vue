<template>
  <div class="home">
    <h1>Hello {{ user.name }}</h1>
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
        workouts: [],
        user: ""
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
        axios.get(`/users/7`).then((response) => {console.log(response)
        this.user = response.data;
        });
      },
    },
  };
</script>
