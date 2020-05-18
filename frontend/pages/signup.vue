<template>

<div class="uk-container">
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
      email: '',
      username: '',
      password: ''
    }
  },
  methods: {
  createUser() {
    axios
      .post('http://localhost:1337/users', {
        username: this.username,
        email: this.email,
        password: this.password,
      })
      .then(response => {
        // Handle success.
        console.log('Well done!');
        console.log('User profile', response);
        console.log('User token', response.data);

        this.loginUser()

        this.$router.push('/')
      })
      .catch(error => {
        // Handle error.
        console.log('An error occurred:', error);
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
        console.log(response)
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>
