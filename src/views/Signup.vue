<template>
<body>
  <div class="signin">
    <!-- <form v-on:submit.prevent="submit()">
      <h1>Signup</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Name:</label>
        <input type="text" v-model="newUserParams.name" />
      </div>
      <div>
        <label>Email:</label>
        <input type="email" v-model="newUserParams.email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="newUserParams.password" />
      </div>
      <div>
        <label>Password confirmation:</label>
        <input type="password" v-model="newUserParams.password_confirmation" />
      </div>
      <input type="submit" value="Submit" />
    </form> -->

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
                <input class="sigin-input" type="email" placeholder="Name" v-model="newUserParams.name"/>
              <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
              <br/>
                <input class="sigin-input" type="email" placeholder="Email" v-model="newUserParams.email"/>
              </div>
              </div>
              <!--Password-->
              <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
                </br>
                <input class="sigin-input" type="password" placeholder="Password" v-model="newUserParams.password"/>
              </div>
              <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
                <br/>
                <input class="sigin-input" type="password" placeholder="Confirm Password" v-model="newUserParams.password_confirmation"/>
              </div>
            </div>
            <br/>
            <div class="sign-up-push">
              <p>Already have an account?</p>
              <a id="a-signup"href="/login">Sign In</a>
            </div>
          </form>
        </div><!--/.form-section-->
        <div class="workout-text-center">
          <button v-on:click="submit()" class="sign-in-button btn btn-outline-secondary">Create Account</button>
        </div>
  </div>
</body>
  


  
</template>

<script>
  import axios from "axios";

  export default {
    data: function () {
      return {
        newUserParams: {},
        errors: [],
      };
    },
    methods: {
      submit: function () {
        axios
          .post("/users", this.newUserParams)
          .then((response) => {
            console.log(response.data);
            this.$router.push("/login");
          })
          .catch((error) => {
            this.errors = error.response.data.errors;
          });
      },
    },
  };
</script>
