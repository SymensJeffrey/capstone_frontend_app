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
      {{julTotal}}-{{augTotal}}
      <div id="container" style="width:100%; height:400px;"></div>
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
        totals: [],
        julTotal: 0,
        augTotal: 0,
      };
    },
    created: function () {
      this.workoutIndex();
      this.userShow();
    },
    methods: {
      workoutIndex: function() {
        axios.get("/workouts").then((response) => {console.log("workout index",   response); 
          this.workouts = response.data.workout;
          this.totals = response.data.total;
          this.julTotal = this.totals["2021-07-01 00:00:00 UTC"]
          this.augTotal = this.totals["2021-08-01 00:00:00 UTC"]
        });
      },
      userShow: function() {
        axios.get(`/users/${localStorage.user_id}`).then((response) => {console.log(response)
        this.user = response.data;
        });
      },
    },
  };
  document.addEventListener('DOMContentLoaded', function () {
    const chart = Highcharts.chart('container', {
      chart: {
          type: 'bar'
      },
      title: {
          text: 'Monthly Data'
      },
      xAxis: {
          categories: ['Jul', 'Aug', 'Sep']
      },
      yAxis: {
          title: {
              text: 'Fruit eaten'
          }
      },
      series: [{
          name: 'Jane',
          data: [2, 0, 4]
      }]
    });
  });
</script>
