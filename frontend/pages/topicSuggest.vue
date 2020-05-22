<template>
  <div class="uk-container uk-container-large uk-position-relative">
    <div class="uk-grid-margin uk-grid uk-grid-stack" uk-grid>
      <div class="uk-width-1-1@m">
        <div class="uk-margin uk-width-large uk-margin-auto">

          <div class="uk-alert-success" v-if="this.success" uk-alert>
              <a class="uk-alert-close" uk-close></a>
              <p>Successfully created a topic suggestion, an admin will review this soon.</p>
          </div>

          <form @submit.prevent="submitted()">
            <fieldset class="uk-fieldset">

                <h1 class="uk-heading-medium uk-text-center">Suggest a Topic!</h1>

                <hr class="uk-margin-medium">

                <div class="uk-margin uk-text-center">
                    <input class="uk-input uk-form-width-large" type="text" placeholder="Topic Name" name="name" v-model="thread_name">
                </div>

                <div class="uk-text-center">
                  Topic Image:
                </div>

                <div class="uk-text-center">
                  <input type="file" ref="image" accept="image/*">
                </div>

                <hr class="uk-margin-medium">

                <div class="uk-margin uk-text-center">
                  <button class="uk-button uk-button-primary uk-button-large" @click="createThread()" type="submit">Suggest</button>
                  <button class="uk-button uk-button-primary uk-button-large" @click="returnHome()" type="submit">Return Home</button>
                </div>

            </fieldset>
          </form>
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
      api_url: process.env.strapiBaseUri,
      thread_name: '',
      success: false,
      file: {}
    }
  },
  methods: {
    submitted() {
      this.success = true;
    },
    createThread() {
      const formElement = document.querySelector('form');
      let data = {
        name: this.thread_name,
        user: this.$auth.user,
        admin_accepted: false,
      }

      axios.post('http://localhost:1337/threads', data)
          .then(res => {
              console.log(res);
              return res.data.id;
          })
          .then(refId =>{
              const data = new FormData();
              data.append('files', formElement.elements[2].files[0]);
              data.append('refId', refId);
              data.append('ref', 'thread');
              data.append('source', 'file');
              data.append('field', 'Image');
              return axios.post('http://localhost:1337/upload', data)
          })
          .then(res =>{
              console.log(res);
              console.log("You suggest a thread successfully...");
          })
          .catch(error =>{
              console.log(error);
          })
    },
    returnHome() {
      this.$router.push("/");
    }
  }
}
</script>
