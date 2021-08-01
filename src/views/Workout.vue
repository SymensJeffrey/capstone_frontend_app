<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      {{ lifts }}
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
