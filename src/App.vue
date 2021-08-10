<template>
  <div id="app">
    <!-- OLD NAV BAR
      <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/workout">Workout</router-link> | 
      <router-link v-if="!isLoggedIn()" to="/signup">Signup</router-link> | 
      <router-link v-if="!isLoggedIn()" to="/login">Login</router-link> 
      <router-link v-if="isLoggedIn()" to="/logout">Logout</router-link> |
      <router-link v-if="isLoggedIn()" to="/profile">Profile</router-link>
      change to {{user.name}} when making dynamic </div> --> 

    <header id="header" class="fixed-top">
      <div class="container d-flex align-items-center justify-content-between">

        <h1 class="logo"><a href="/">GYMOLOGY</a></h1>
        <!-- Uncomment below if you prefer to use an image logo -->
        <!-- <a href="index.html" class="logo"><img src="assets/img/logo.png" alt="" class="img-fluid"></a> -->

        <nav id="navbar" class="navbar">
          <ul>
            <li><a class="active" href="/">Home</a></li>
            <li><a href="/workout">Workout</a></li>
            <li v-if="isLoggedIn()"><a href="/profile">Profile</a></li>
            <li v-if="!isLoggedIn()"><a href="/login">Login</a></li>
            <li v-if="!isLoggedIn()"><a href="/signup">Signup</a></li>
            <!-- <li class="dropdown"><a href="#"><span>Drop Down</span> <i class="bi bi-chevron-down"></i></a>
              <ul>
                <li><a href="#">Drop Down 1</a></li>
                <li class="dropdown"><a href="#"><span>Deep Drop Down</span> <i class="bi bi-chevron-right"></i></a>
                  <ul>
                    <li><a href="#">Deep Drop Down 1</a></li>
                    <li><a href="#">Deep Drop Down 2</a></li>
                    <li><a href="#">Deep Drop Down 3</a></li>
                    <li><a href="#">Deep Drop Down 4</a></li>
                    <li><a href="#">Deep Drop Down 5</a></li>
                  </ul>
                </li>
                <li><a href="#">Drop Down 2</a></li>
                <li><a href="#">Drop Down 3</a></li>
                <li><a href="#">Drop Down 4</a></li>
              </ul>
            </li> -->
            <li v-if="isLoggedIn()"><a href="/logout">Logout</a></li>
          </ul>
          <i class="bi bi-list mobile-nav-toggle"></i>
        </nav><!-- .navbar -->

      </div>
    </header>
    <router-view/>
    <footer id="footer">
      <div class="container">
        <h3>GYMOLOGY</h3>
        <p>Et aut eum quis fuga eos sunt ipsa nihil. Labore corporis magni eligendi fuga maxime saepe commodi placeat.</p>
        <div class="social-links">
          <a href="#" class="twitter"><i class="bx bxl-twitter"></i></a>
          <a href="#" class="facebook"><i class="bx bxl-facebook"></i></a>
          <a href="#" class="instagram"><i class="bx bxl-instagram"></i></a>
          <a href="#" class="google-plus"><i class="bx bxl-skype"></i></a>
          <a href="#" class="linkedin"><i class="bx bxl-linkedin"></i></a>
        </div>
        <div class="copyright">
          &copy; Copyright <strong><span>MeFamily</span></strong>. All Rights Reserved
        </div>
        <div class="credits">
          Designed by <a href="https://bootstrapmade.com/">BootstrapMade</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<style>

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

