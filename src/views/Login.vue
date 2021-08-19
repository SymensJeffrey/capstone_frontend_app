<template>
  <div class="login">
    <br>
    <br>
    <br>
    <br>
    <body>
      <div class="signin">
        <div class="back-img">
          <div class="sign-in-text">
          </div><!--/.sign-in-text-->
          <div class="layer">
          </div><!--/.layer-->
        </div><!--/.back-img-->
        <div class="form-section">
        
          <form v-on:submit.prevent="submit()">
            <!--Email-->
            <div class="float-center">
              <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
                <ul>
                  <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
                </ul>
                <input class="sigin-input" type="email" placeholder="Email" v-model="newSessionParams.email">
              </div>
              <br/>
              <br/>
              <!--Password-->
              <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
                <input class="sigin-input" type="password" placeholder="Password" v-model="newSessionParams.password">
              </div>
            </div>
            <br/>
            <br/>
            <div class="sign-up-push">
              <p>Dont have and account?</p>
              <a id="a-signup"href="/signup">Sign Up</a>
            </div>
          </form>
        </div><!--/.form-section-->
        <div class="workout-text-center">
          <button v-on:click="submit()" class="sign-in-button btn btn-outline-secondary">Sign in</button>
        </div>
        <!--/button-->
      </div><!--/.signin-->
    </body>
  </div>
  
</template>

<script>
  import axios from "axios";

  export default {
    data: function () {
      return {
        newSessionParams: {},
        errors: [],
      };
    },
    methods: {
      submit: function () {
        axios
          .post("/sessions", this.newSessionParams)
          .then((response) => {
            axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
            localStorage.setItem("jwt", response.data.jwt);
            localStorage.setItem("user_id", response.data.user_id);
            this.$router.push("/");
          })
          .catch((error) => {
            console.log(error.response);
            this.errors = ["Invalid email or password."];
            this.email = "";
            this.password = "";
          });
      },
      pushToSignup: function() {
        this.$router.push("/signup");
      }
    },
  };
</script>
