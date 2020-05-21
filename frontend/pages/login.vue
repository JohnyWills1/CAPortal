<template>

<div>
  <div class="uk-container">
    <!-- Alert if form is empty -->
    <div v-if="errors.length" v-for="error in errors" class="uk-alert-danger" uk-alert>
      <a class="uk-alert-close" uk-close></a>
      <p>{{ error }}</p>
    </div>
    <div v-if="show_invalid" class="uk-alert-danger" uk-alert>
      <a class="uk-alert-close" uk-close></a>
      <p>{{ invalid_message }}</p>
    </div>

    <div class="uk-grid-margin uk-grid uk-grid-stack" uk-grid>
      <div class="uk-width-1-1@m">
        <div class="uk-margin uk-width-large uk-margin-auto">
          <div class="uk-modal-header">
            <h3 class="uk-card-title uk-text-center">Welcome back!</h3>
          </div>

          <div class="uk-modal-body">
            <form @submit.stop.prevent="loginUser()">
              <div class="uk-margin">
                <div class="uk-inline uk-width-1-1">
                  <span class="uk-form-icon" uk-icon="mail"></span>
                  <input class="uk-input uk-form-large" type="text" placeholder="E-mail" v-model="login.identifier">
                </div>
              </div>

              <div class="uk-margin">
                <div class="uk-inline uk-width-1-1">
                  <span class="uk-form-icon" uk-icon="lock"></span>
                  <input class="uk-input uk-form-large" type="password" placeholder="Password" v-model="login.password">
                </div>
              </div>

              <div class="uk-modal-footer uk-text-center">
                <div class="uk-margin">
                  <button class="uk-button uk-button-primary uk-button-large uk-width-1-1" type="submit">Login</button>
                </div>
                <div class="uk-text-small uk-text-center">Not registered? <a href="/signup"> Create an account</a></div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      errors: [],
      show_invalid: false,
      invalid_message: '',
      //Log In
      login: {
        identifier: '',
        password: ''
      }
    }
  },
  methods: {
    async loginUser() {
      try {

        //Check form for errors
        this.checkForm()

        let response = await this.$auth.loginWith('local', { data: this.login })
        // this.$router.push("/");
        axios
          .post('http://localhost:1337/auth/local', {
            identifier: this.login.identifier,
            password: this.login.password,
          })
          .then(response => {
            // Handle success.
            console.log('Well done!');
            console.log('User profile', response.data.user);
            console.log('User token', response.data.jwt);
          })
          .catch(error => {
            // Handle error.
            console.log('An error occurred:', error);
          });
      } catch (err) {
        this.show_invalid = true;
        this.invalid_message = err.response.data.message[0].messages[0].message;
      }
    },
    checkForm() {
      if (this.login.identifier && this.login.password){
        return true
      }
      this.errors = [];

      if (!this.login.identifier) {
        this.errors.push("E-mail is required.")
      }
      if (!this.login.password) {
        this.errors.push("Password is required.")
      }
      this.show_invalid = false;
    }
  }
}
</script>
