<template>
  <div class="home">
    <br>
    <br>
    <section id="hero">
      <div id="heroCarousel" data-bs-interval="5000" class="carousel slide carousel-fade" data-bs-ride="carousel">

        <!-- This gives me an error <ol class="carousel-indicators" id="hero-carousel-indicators"></ol> -->

        <div class="carousel-inner" role="listbox">

          <!-- Slide 1 -->
          <div class="carousel-item active" style="background-image: url(assets/img/slide/slide-1.jpg)">
          </div>

          <!-- Slide 2 -->
          <div class="carousel-item" style="background-image: url(assets/img/slide/slide-2.jpeg)">
          </div>

          <!-- Slide 3 -->
          <div class="carousel-item" style="background-image: url(assets/img/slide/slide-3.jpg)">
          </div>

        </div>

        <a class="carousel-control-prev" href="#heroCarousel" role="button" data-bs-slide="prev">
          <span class="carousel-control-prev-icon bi bi-chevron-left" aria-hidden="true"></span>
        </a>

        <a class="carousel-control-next" href="#heroCarousel" role="button" data-bs-slide="next">
          <span class="carousel-control-next-icon bi bi-chevron-right" aria-hidden="true"></span>
        </a>

      </div>
    </section><!-- End Hero -->
    <hr />
    <div v-for="exercise in exercises">
      <h6>{{ exercise.name }}</h6>
      <button v-on:click="exerciseShow(exercise)"> More Info </button>
      <button v-on:click="addToWorkoutPopup(exercise)">Add to workout</button> 
      <hr />
    </div>
    <dialog id="exercise-details">
      <form method="dialog">
        <h2>{{ currentExercise.name }}</h2>
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
        newLiftParams: {},
        currentUser: {},
        lifts: [],
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
        var LiftParams = {
          reps: this.newLiftParams.reps,
          weight: this.newLiftParams.weight,
          sets: this.newLiftParams.sets,
          user_id: localStorage.getItem("user_id"),
          exercise_id: this.currentExercise.id
        };
        axios.post("/lifts", LiftParams).then(response => {
          console.log(response.data);
          this.lifts.push(response.data);
          this.newLiftParams = {};
        })
      },
    },
  };
</script>