<template>

<div>
  <div class="uk-container">
    <div class="uk-grid-margin uk-grid uk-grid-stack" uk-grid>
      <div class="uk-width-1-1@m">
        <div class="uk-margin uk-width-large uk-margin-auto">
          <div class="uk-modal-header">
            <h3 class="uk-card-title uk-text-center">Password Reset</h3>
          </div>

          <div class="uk-modal-body">
            <form @submit.stop.prevent="resetPassword()">
              <div class="uk-margin">
                <div class="uk-inline uk-width-1-1">
                  <span class="uk-form-icon" uk-icon="mail"></span>
                  <input class="uk-input uk-form-large" type="text" v-model.lazy="login.email">
                </div>
              </div>

              <div class="uk-modal-footer uk-text-center">
                <div class="uk-margin">
                  <button class="uk-button uk-button-primary uk-button-large uk-width-1-1" type="submit">Login</button>
                </div>
                <div class="uk-text-small uk-text-center">Not registered? <a href="/signup"> Create an account</a></div>
                <div class="uk-text-small uk-text-center">Forgot Password? <a onclick="resetPassword()"> Send Reset E-mail</a></div>
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
      login: {
        email: ''
      }
    }
  },
  methods: {
    resetPassword() {
      axios
        .post('http://localhost:1337/auth/forgot-password', {
          email: this.login.email,
        })
        .then(response => {
          // Handle success.
          console.log('Your user received an email');
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
        });
    }
  }
}
</script>
