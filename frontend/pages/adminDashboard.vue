<template>
  <div class="uk-section">
    <div class="uk-container uk-container-small uk-position-relative uk-text-center">

      <h1 class="uk-heading-large uk-text-center">Admin Dashboard</h1>
      <hr class="uk-margin-medium">

      <h2 class="uk-heading-large uk-text-center">Users</h2>
      <p>Block users, promote users to moderators or delete threads:</p>
      <hr class="uk-margin-small">
      <!-- User table here -->
      <table class="uk-table uk-table-divider">
          <thead>
              <tr>
                  <th>Username</th>
                  <th>E-mail</th>
                  <th>Blocked</th>
                  <th>Role</th>
                  <th>Action</th>
              </tr>
          </thead>
          <tbody class="uk-text-left">
              <tr v-for="user in users" v-if="user.role.name != 'Admin' ">
                  <td>{{ user.username }}</td>
                  <td>{{ user.email }}</td>
                  <td>{{ user.blocked }}</td>
                  <td>{{ user.role.name }}</td>
                  <td>
                    <button class="uk-button-primary" @click="ban(user.id)">Ban</button>
                    <button class="uk-button-primary" @click="promote(user.id)">Promote</button>
                  </td>
              </tr>
          </tbody>
      </table>
      <hr class="uk-margin-medium">

      <h2 class="uk-heading-large uk-text-center">Threads</h2>
      <table class="uk-table uk-table-divider">
          <thead>
              <tr>
                  <th>Thread Name</th>
                  <th>Status</th>
                  <th>User</th>
              </tr>
          </thead>
          <tbody class="uk-text-left">
              <tr v-for="thread in threads" v-if="!thread.admin_accepted">
                <td>{{ thread.name }}</td>
                <td>{{ thread.admin_accepted }}</td>
                <td>{{ thread.user.username }}</td>
                <td>
                  <button class="uk-button-primary" @click="accept(thread.id)">Accept</button>
                  <button class="uk-button-primary" @click="reject(thread.id)">Reject</button>
                </td>
            </tr>
          </tbody>
      </table>

    </div>
  </div>
</template>

<script>
import usersQuery from '~/apollo/queries/user/users';
import threadsQuery from '~/apollo/queries/thread/threads';
import axios from 'axios';

export default {
  data() {
    return {
      users: [],
      threads: [],
      api_url: process.env.strapiBaseUri
    }
  },
  apollo: {
    threads: {
      prefetch: true,
      query: threadsQuery
    },
    users: {
      prefetch: true,
      query: usersQuery
      }
  },
  methods: {
    ban(id) {
      console.log(id)
      axios
        .put('http://localhost:1337/users/' + id, {
          blocked: true
        })
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
        });
    },
    promote(id) {
      axios
        .put('http://localhost:1337/users/' + id, {
          role: {id:6,name:"Moderator",type:"moderator",__typename:"UsersPermissionsRole"}
        })
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
        });
    },
    accept (id) {
      axios
        .put('http://localhost:1337/threads/' + id, {
          admin_accepted: true
        })
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
        });
    },
    reject (id) {
      axios
        .delete('http://localhost:1337/threads/' + id)
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          // Handle error.
          console.log('An error occurred:', error);
        });
    }
  }
}
</script>
