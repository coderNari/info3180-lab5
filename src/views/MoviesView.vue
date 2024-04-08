<template>
  <div class="movies-container">
    <h1>Movies</h1>
    <div class="movie-card" v-for="movie in movies" :key="movie.id">
      <img :src="movie.poster" alt="Movie Poster" class="movie-image" />
      <div class="movie-details">
        <h2>{{ movie.title }}</h2>
        <p>{{ movie.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup> 
import { ref, onMounted } from "vue"; 
let movies = ref([]); 
const fetchMovies = () => {
  fetch("/api/v1/movies")
    .then(response => response.json())
    .then(data => {
      movies.value = data.movies;
    })
    .catch(error => {
      console.error('Error fetching movies:', error);
    });
}
onMounted(() => {
  fetchMovies();
});
</script>

<style scoped>
.movies-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr); /* 2 columns */
  gap: 20px; /* gap between movie cards */
}

.movie-card {
  display: flex;
  align-items: center;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.movie-image {
  max-width: 150px; /* Adjust image width as needed */
  margin-right: 20px; /* Spacing between image and details */
}

.movie-details {
  flex: 1; /* Allow title and description to fill remaining space */
}

.movie-details h2 {
  font-weight: bold; /* Bold title */
}
</style>