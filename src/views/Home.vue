<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>

    </div>
  </div>
</template>

<style></style>

<script>
  export default {
    data: function () {
      return {
        message: "GYMOLOGY",
      };
    },
    created: function () {
      this.exerciseIndex()
    },
    methods: {
      exerciseIndex: function() {
        console.log("indexing...")
      },
    },
  };
</script>
