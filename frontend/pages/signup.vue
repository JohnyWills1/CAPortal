<template>
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
          <h3 class="uk-card-title uk-text-center">Create An Account</h3>
        </div>
        <div class="uk-modal-body">
          <form @submit.prevent="createUser()" method="post" v-if="!$store.state.authUser">
            <div class="uk-margin">
              <div class="uk-inline uk-width-1-1">
                <span class="uk-form-icon" uk-icon="mail"></span>
                <input class="uk-input uk-form-large" type="email" placeholder="E-mail" v-model="email" />
              </div>
            </div>

            <div class="uk-margin">
              <div class="uk-inline uk-width-1-1">
                <span class="uk-form-icon" uk-icon="user"></span>
                <input class="uk-input uk-form-large" type="text" placeholder="Username" v-model="username" />
              </div>
            </div>

            <div class="uk-margin">
              <div class="uk-inline uk-width-1-1">
                <span class="uk-form-icon" uk-icon="lock"></span>
                <input class="uk-input uk-form-large" type="password" placeholder="Password" v-model="password" />
              </div>
            </div>

            <div class="uk-modal-footer uk-text-center">
              <div class="uk-margin">
                <button class="uk-button uk-button-primary uk-button-large uk-width-1-1" type="submit">Sign Up</button>
              </div>
            </div>
          </form>
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
      //Sign Up
      errors: [],
      show_invalid: false,
      invalid_message: '',
      email: '',
      username: '',
      password: ''
    }
  },
  methods: {
  createUser() {
    this.show_invalid = false;

    //Check the form
    this.checkForm()

    axios
      .post('http://localhost:1337/auth/local/register', {
        username: this.username,
        email: this.email,
        password: this.password,
      })
      .then(response => {
        // Handle success.
        this.loginUser()

        this.$router.push('/')
      })
      .catch(error => {
        // Handle error.
        this.show_invalid = true;
        this.invalid_message = error.response.data.message[0].messages[0].message;
      });
    },
    async loginUser() {
      try {
        let response = await this.$auth.loginWith('local', {
          data: {
              identifier: this.email,
              password: this.password
          }
        })
        //Success
      } catch (err) {
        //Error
      }
    },
    checkForm() {
      if (this.email && this.password && this.email){
        return true
      }
      this.errors = [];

      if (!this.email) {
        this.errors.push("E-mail is required.")
      }
      if (!this.password) {
        this.errors.push("Password is required.")
      }
      if (!this.username) {
        this.errors.push("Username is required.")
      }

    }
  }
}
</script>
