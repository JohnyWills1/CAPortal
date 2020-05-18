<template>

<div>
  <div class="uk-container">
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
                <div class="uk-text-small uk-text-center">Forgot Password? <a href="/resetPassword"> Send Reset E-mail</a></div>
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
        let response = await this.$auth.loginWith('local', { data: this.login })
        console.log(response)
        console.log(response.data)
        // this.$router.push("/");
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>
