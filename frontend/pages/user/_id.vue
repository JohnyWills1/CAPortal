<template>
  <div class="uk-section">
    <h1 class="uk-heading-large uk-text-center">User Settings</h1>
    <div class="uk-container uk-container-small uk-position-relative uk-text-center">

      <div class="uk-alert-success" v-if="this.success" uk-alert>
          <a class="uk-alert-close" uk-close></a>
          <p>Successfully changed user {{ this.changed_attr }}</p>
      </div>

      <div class="uk-alert-danger" v-if="this.error" uk-alert>
          <a class="uk-alert-close" uk-close></a>
          <p>Failed to change user {{ this.changed_attr }}</p>
      </div>

      <hr class="uk-margin-medium">
      <table class="uk-table uk-table-striped">
          <thead>
              <tr>
                  <th>Username</th>
                  <th>E-mail</th>
                  <th>Role</th>
              </tr>
          </thead>
          <tbody class="uk-text-left">
              <tr>
                  <td>{{ $auth.user.username }}</td>
                  <td>{{ $auth.user.email }}</td>
                  <td>{{ $auth.user.role.name }}</td>
              </tr>
          </tbody>
      </table>
      <div class="uk-margin">
          <p>Change Username: </p>
          <input class="uk-input uk-form-width-large" type="text" :placeholder="'Current Username: ' + $auth.user.username" v-model="chng_username">
          <button class="uk-button uk-button-secondary uk-padding-top" @click="cUsername()">Change Username</button>
      </div>
      <div class="uk-margin">
          <p>Change E-mail: </p>
          <input class="uk-input uk-form-width-large" type="text" :placeholder="'Current E-mail: ' + $auth.user.email" v-model="chng_email">
          <button class="uk-button uk-button-secondary uk-padding-top" @click="cEmail()"> Change E-mail </button>
      </div>
      <div class="uk-margin">
          <p>Change Password: </p>
          <input class="uk-input uk-form-width-large" type="password" placeholder="Password" v-model="chng_password">
          <button class="uk-button uk-button-secondary uk-padding-top" @click="cPassword()">Change Password</button>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      api_url: process.env.strapiBaseUri,
      chng_username: '',
      chng_email: '',
      chng_password: '',
      changed_attr: '',
      success: false,
      error: false
    }
  },
  methods: {
    cUsername() {
      axios
        .put('http://localhost:1337/users/' + this.$auth.user.id, {
          username: this.chng_username
        })
        .then(response => {
          console.log(response)
          this.changed_attr = "Username";
          this.success = true;
          this.$router.push("/user/" + this.$auth.user.id)
        })
        .catch(error => {
          // Handle error.
          this.changed_attr = "Username";
          this.error = true;
          console.log('An error occurred:', error);
        });
    },
    cEmail() {
      axios
        .put('http://localhost:1337/users/' + this.$auth.user.id, {
          email: this.chng_email
        })
        .then(response => {
          console.log(response)
          this.changed_attr = "Email";
          this.success = true;
          this.$router.push("/user/" + this.$auth.user.id)
        })
        .catch(error => {
          // Handle error.
          this.changed_attr = "Email";
          this.error = true;
          console.log('An error occurred:', error);
        });
    },
    cPassword() {
      axios
        .put('http://localhost:1337/users/' + this.$auth.user.id, {
          password: this.chng_password
        })
        .then(response => {
          console.log(response)
          this.changed_attr = "Password";
          this.success = true;
          this.$router.push("/user/" + this.$auth.user.id)
        })
        .catch(error => {
          // Handle error.
          this.changed_attr = "Password";
          this.error = true;
          console.log('An error occurred:', error);
        });
    }
  }
}
</script>
