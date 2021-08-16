<template>
  <div class="home black-background">
    <br>
    <br>
    <section id="hero">
      <div id="heroCarousel" data-bs-interval="5000" class="carousel slide carousel-fade" data-bs-ride="carousel">

        <!-- This gives me an error <ol class="carousel-indicators" id="hero-carousel-indicators"></ol> -->

        <div class="carousel-inner" role="listbox">

          <!-- Slide 1 -->
          <div class="carousel-item active" id="logo" style="background-image: url(assets/img/slide/slide-1.jpg)">
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
    <div class="container white-background">
    <section id="features" class="features">
      <div class="search-bar-padding">
        <p><input id="search-bar" placeholder="Search" v-model="searchTerm"></p>
      </div>
      <div class="row g-5">
        <div class="col-md-4" v-for="exercise in filterBy(exercises, searchTerm, 'name')">
            <div class="icon-box">
              <div class="icon">
                <img class id="body-part" v-bind:src="exercise.image_url">
            </div>
            <h4 class="title"><a v-on:click="exerciseShow(exercise)">{{ exercise.name }}</a></h4>
            <p class="description">
              <button v-if="isLoggedIn()" v-on:click="addToWorkoutPopup(exercise)" class="btn btn-outline-secondary round-button">Add to workout</button>
            </p>
            </div>
        </div>
      </div>
      <dialog id="exercise-details">
          <form method="dialog">
            <h2>{{ currentExercise.name }}</h2>
            <p>Description:{{ currentExercise.description }}</p>
            <p>Equipment:{{ currentExercise.equipment }}</p>
            <p>Muscles:{{ currentExercise.category }}</p>
            <div class="vertical-center">
              <button class="btn btn-outline-secondary round-button">Close</button>
            </div>
          </form>
      </dialog>
      <dialog id="lift-create">
        <form method="dialog" class="float-center">
          <h2>Adding {{ currentExercise.name }}</h2>
          <p><input type="number" v-model="newLiftParams.reps" class="new-workout-input">Reps</p>
          <p><input type="number" v-model="newLiftParams.weight" class="new-workout-input"> Weight</p>
          <p><input type="number" v-model="newLiftParams.sets" class="new-workout-input"> Sets</p>
          <div class="vertical-center">
            <button v-on:click="liftCreate()" class="btn btn-outline-secondary round-button">Add to Workout</button>
            <button class="btn btn-outline-secondary round-button">Close</button>
          </div>
        </form>
      </dialog>
    </section>
  </div>
  </div>
</template>

<style>
#body-part {
  width: 60px
}
</style>

<script>
  import axios from "axios"
  import Vue2Filters from "vue2-filters"
  export default {
    mixins: [Vue2Filters.mixin],
    data: function () {
      return {
        message: "GYMOLOGY",
        exercises: [],
        currentExercise: {},
        newLiftParams: {},
        currentUser: {},
        lifts: [],
        searchTerm: ""
      };
    },
    created: function () {
      this.exercisesIndex();
      this.isLoggedIn();
      this.userShow();
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
      isLoggedIn: function() {
      if (localStorage.getItem("jwt")) {
        return true;
      } else {
        return false;
      }      
      },
      userShow: function() {
        axios.get(`/users/${localStorage.user_id}`).then((response) => {console.log(response)
        this.user = response.data;
        });
      },
    },
  };
</script>