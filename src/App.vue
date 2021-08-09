<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/workout">Workout</router-link> | 
      <router-link v-if="!isLoggedIn()" to="/signup">Signup</router-link> | 
      <router-link v-if="!isLoggedIn()" to="/login">Login</router-link> |
      <router-link v-if="isLoggedIn()" to="/logout">Logout</router-link> |
      <router-link v-if="isLoggedIn()" to="/profile">Profile</router-link>
      <!-- change to {{user.name}} when making dynamic -->
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>
import axios from "axios"
  export default {
    data: function () {
      return {
        message: "Welcome to Vue.js!",
        user: "",
      };
    },
    created: function () {
      this.userShow()
      this.isLoggedIn()
    },
    methods: {
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

