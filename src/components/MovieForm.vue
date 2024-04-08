<template>
  <form @submit.prevent="saveMovie" id="movieForm">
    <div class="form-group mb-3">
      <label for="title" class="form-label">Movie Title</label>
      <input type="text" v-model="formData.title" name="title" class="form-control" />
    </div>
    <div class="form-group mb-3">
      <label for="description" class="form-label">Description</label>
      <textarea v-model="formData.description" name="description" class="form-control"></textarea>
    </div>
    <div class="form-group mb-3">
      <label for="poster" class="form-label">Movie Poster</label>
      <input type="file" @change="handleFileUpload" name="poster" class="form-control" />
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  data() {
    return {
      formData: {
        title: '',
        description: '',
        poster: null
      },
      csrf_token: "", // Initialize csrf_token as a data property
    };
  },
  methods: {
    saveMovie() {
      let movieForm = document.getElementById('movieForm'); 
      let form_data = new FormData(movieForm); 

      fetch("/api/v1/movies", { 
        method: 'POST',
        body: form_data, 
        headers: {
          'X-CSRFToken': this.csrf_token // Use this.csrf_token to access the csrf_token
        }
      })
      .then(function (response) { 
        return response.json(); 
      })
      .then(function (data) { 
        // Display a success message 
        console.log(data); 
      })
      .catch(function (error) { 
        console.log(error); 
      });
    },
    handleFileUpload(event) {
      this.formData.poster = event.target.files[0];
    },
    getCsrfToken() {
      fetch('/api/v1/csrf-token')
      .then(response => response.json())
      .then(data => {
        console.log(data);
        this.csrf_token = data.csrf_token; // Assign the csrf_token to this.csrf_token
      })
      .catch(error => {
        console.error(error);
      });
    }
  },
  mounted() { // Use the mounted hook instead of onMounted
    this.getCsrfToken(); // Call getCsrfToken when the component is mounted
  }
};
</script>
