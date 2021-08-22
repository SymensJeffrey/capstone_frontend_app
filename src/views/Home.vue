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
      <div class="workout-text-center">
        <button id="arm" v-on:click="changeCSSarm(),showArms()" class="btn btn-outline-secondary">Arms</button>
        <button id="back" v-on:click="changeCSSback(),showBack()" class="btn btn-outline-secondary">Back</button>
        <button id="leg" v-on:click="changeCSSleg(),showLegs()" class="btn btn-outline-secondary">Legs</button>
        <button id="chest" v-on:click="changeCSSchest(),showChest()" class="btn btn-outline-secondary">Chest</button>
        <button id="abs" v-on:click="changeCSSabs(),showAbs()" class="btn btn-outline-secondary">Abs</button>
        <button id="calves" v-on:click="changeCSScalves(),showCalves()" class="btn btn-outline-secondary">Calves</button>
      </div>
      <br>
      <br>
      <div class="row g-5">
        <div data-aos="slide-up" class="col-md-4" v-for="exercise in filterBy(exercises, searchTerm  || muscleGroup, 'name', 'muscle')">
            <div class="icon-box">
              <div class="icon">
                <img class id="body-part" v-bind:src="exercise.image_url">
            </div>
            <h4 class="title"><a v-on:click="exerciseShow(exercise)">{{ exercise.name }}</a></h4>
            <p class="description">
              <button v-if="isLoggedIn()" v-on:click="addToWorkoutPopup(exercise)" class="btn btn-outline-secondary">Add to workout</button>
            </p>
            </div>
        </div>
      </div>
      <dialog id="exercise-details">
          <form method="dialog">
            <h2 class="vertical-center">{{ currentExercise.name }}</h2>
            <p><h5 class="exercise-info-titles">Description:</h5>{{ currentExercise.description }}</p>
            <p><h5 class="exercise-info-titles">Muscles:</h5>{{ currentExercise.muscle }}</p>
            <div class="vertical-center">
              <button class="btn btn-outline-secondary">Close</button>
            </div>
          </form>
      </dialog>
      <dialog id="lift-create">
        <form method="dialog">
          <div class="input-content">
            <h2>{{ currentExercise.name }}</h2>
            <p>
                <button v-on:click.prevent="Default()" v-on:click="newLiftParams.reps -= 1" class="btn up-down-button" data-toggle="">-</button>
                <button v-on:click.prevent="Default()" v-on:click="newLiftParams.reps += 1" class="btn up-down-button" data-toggle="">+</button>
              <input type="number" v-model="newLiftParams.reps" class="new-workout-input">Reps
            </p>
            <p>
              <button v-on:click.prevent="Default()" v-on:click="newLiftParams.weight -= 5" class="btn up-down-button" data-toggle="">-</button>
              <button v-on:click.prevent="Default()" v-on:click="newLiftParams.weight += 5" class="btn up-down-button" data-toggle="">+</button>
              <input type="number" v-model="newLiftParams.weight" class="new-workout-input"> Weight
            </p>
            <p>
              <button v-on:click.prevent="Default()" v-on:click="newLiftParams.sets -= 1" class="btn up-down-button" data-toggle="">-</button>
              <button v-on:click.prevent="Default()" v-on:click="newLiftParams.sets += 1" class="btn up-down-button" data-toggle="">+</button>
              <input type="number" v-model="newLiftParams.sets" class="new-workout-input"> Sets
            </p>
          </div>
          <div class="vertical-center">
            <button v-on:click="liftCreate()" class="btn btn-outline-secondary">Add to Workout</button>
            <button class="btn btn-outline-secondary">Close</button>
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
        newLiftParams: {reps:0, weight:0, sets:0},
        currentUser: {},
        lifts: [],
        searchTerm: "",
        muscleGroup: "",
      };
    },
    created: function () {
      this.exercisesIndex();
      this.isLoggedIn();
      this.userShow();
    },
    methods: {
      exercisesIndex: function() {
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
          this.newLiftParams = {reps:0, weight:0, sets:0}
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
      showArms: function() {
        if (this.muscleGroup == ""){
        this.muscleGroup = "arms"
        } else {
          this.muscleGroup = ""
        }
      },
      showBack: function() {
        if (this.muscleGroup == ""){
        this.muscleGroup = "back"
        } else {
          this.muscleGroup = ""
        }
      },
      showLegs: function() {
        if (this.muscleGroup == ""){
        this.muscleGroup = "legs"
        } else {
          this.muscleGroup = ""
        }
      },
      showChest: function() {
        if (this.muscleGroup == ""){
        this.muscleGroup = "chest"
        } else {
          this.muscleGroup = ""
        }
      },
      showAbs: function() {
        if (this.muscleGroup == ""){
        this.muscleGroup = "abs"
        } else {
          this.muscleGroup = ""
        }
      },
      showCalves: function() {
        if (this.muscleGroup == ""){
        this.muscleGroup = "calf"
        } else {
          this.muscleGroup = ""
        }
      },
      changeCSSarm: function() {
        if (this.muscleGroup == ""){
          document.getElementById('arm').setAttribute("class", "btn btn-outline-secondary button-active");
        } else {
          document.getElementById('arm').setAttribute("class", "btn btn-outline-secondary");
        }
      },
      changeCSSback: function() {
        if (this.muscleGroup == ""){
          document.getElementById('back').setAttribute("class", "btn btn-outline-secondary button-active");
        } else {
          document.getElementById('back').setAttribute("class", "btn btn-outline-secondary");
        }
      },
      changeCSSleg: function() {
        if (this.muscleGroup == ""){
          document.getElementById('leg').setAttribute("class", "btn btn-outline-secondary button-active");
        } else {
          document.getElementById('leg').setAttribute("class", "btn btn-outline-secondary");
        }
      },
      changeCSSchest: function() {
        if (this.muscleGroup == ""){
          document.getElementById('chest').setAttribute("class", "btn btn-outline-secondary button-active");
        } else {
          document.getElementById('chest').setAttribute("class", "btn btn-outline-secondary");
        }
      },
      changeCSSabs: function() {
        if (this.muscleGroup == ""){
          document.getElementById('abs').setAttribute("class", "btn btn-outline-secondary button-active");
        } else {
          document.getElementById('abs').setAttribute("class", "btn btn-outline-secondary");
        }
      },
      changeCSScalves: function() {
        if (this.muscleGroup == ""){
          document.getElementById('calves').setAttribute("class", "btn btn-outline-secondary button-active");
        } else {
          document.getElementById('calves').setAttribute("class", "btn btn-outline-secondary");
        }
      },
      increaseReps: function() {

        console.log(this.newLiftParams)
      },
      decreaseReps: function() {
        console.log("decrease")
        console.log(this.newLiftParams)
      },
      Default: function () {
      }
    },
  };
</script>