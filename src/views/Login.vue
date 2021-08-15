<template>
  <div class="login">
    <br>
    <br>
    <br>
    <br>
    <!-- <form v-on:submit.prevent="submit()">
      <h1>Login</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Email:</label>
        <input type="email" v-model="newSessionParams.email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="newSessionParams.password" />
      </div>
      <input type="submit" value="Submit" />       
      <button v-on:click="pushToSignup()">Signup</button>
    </form> -->



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
                <!-- <input class="mdl-textfield__input" type="email" id="sample3" v-model="newSessionParams.email"> -->
                <input class="sigin-input" type="email" placeholder="Email" v-model="newSessionParams.email">
              </div>
              <br/>
              <br/>
              <!--Password-->
              <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
                <!-- <input pattern=".{8,}" class="mdl-textfield__input" type="password" id="sample3" v-model="newSessionParams.password"> -->
                <input class="sigin-input" type="password" placeholder="Password" v-model="newSessionParams.password">
              </div>
            </div>
            <br/>
            <br/>
            <br/>
            <div class="sign-up-push">
              <p>Dont have and account?</p>
              <a id="a-signup"href="/signup">Sign Up</a>
            </div>
          </form>
        </div><!--/.form-section-->
        
        <input id="signin-button" v-on:click="submit()" value="Sign In" class="sign-in-btn mdl-button mdl-js-ripple-effect mdl-js-button mdl-button--raised mdl-button--colored workout-text-center"/>
        
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
        console.log("its working")
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
