<template>
  <div class="uk-container uk-container-large uk-position-relative">
    <form @submit.stop.prevent="createThread()">
      <fieldset class="uk-fieldset">

          <h1 class="uk-heading-medium uk-text-center">Suggest a Topic!</h1>

          <hr class="uk-margin-medium">

          <div class="uk-margin uk-text-center">
              <input class="uk-input uk-form-width-large" type="text" placeholder="Topic Name" name="name" v-model="thread_name">
          </div>

          <div class="uk-text-center">
            Topic Image:
          </div>

          <div class="uk-margin uk-text-center">
              <!-- <div uk-form-custom>
                  <input type="file">
                  <button class="uk-button uk-button-default" type="button" tabindex="-1">Upload File</button>
              </div> -->
              <input type="file" name="Image">
          </div>

          <hr class="uk-margin-medium">

          <div class="uk-margin uk-text-center">
            <button class="uk-button uk-button-primary uk-button-large uk-width-1-1" type="submit">Submit</button>
          </div>

      </fieldset>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      api_url: process.env.strapiBaseUri,
      thread_name: ''
    }
  },
  methods: {
    createThread() {
      const formElement = document.querySelector('form');

      formElement.addEventListener('submit', e => {
        e.preventDefault();

        const request = new XMLHttpRequest();

        const formData = new FormData();

        const formElements = formElement.elements;

        const data = {};

        for (let i = 0; i < formElements.length; i++) {
          const currentElement = formElements[i];
          if (!['submit', 'file'].includes(currentElement.type)) {
            data[currentElement.name] = currentElement.value;
          } else if (currentElement.type === 'file') {
            if (currentElement.files.length === 1) {
              const file = currentElement.files[0];
              formData.append(`files.${currentElement.name}`, file, file.name);
            } else {
              for (let i = 0; i < currentElement.files.length; i++) {
                const file = currentElement.files[i];

                formData.append(`files.${currentElement.name}`, file, file.name);
              }
            }
          }
        }

        formData.append('data', JSON.stringify(data));

        request.open('POST', `${this.api_url}/threads`);

        request.send(formData);
      });
    }
  }
}
</script>
