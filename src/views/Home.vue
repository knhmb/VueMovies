<template>
  <div class="home">
    <div class="feature-card">
      <!-- <router-link to="/movie/tt0409591"> -->
      <div
        id="carouselExampleCaptions"
        class="carousel slide"
        data-bs-ride="carousel"
      >
        <div class="carousel-indicators">
          <button
            type="button"
            data-bs-target="#carouselExampleCaptions"
            data-bs-slide-to="0"
            class="active"
            aria-current="true"
            aria-label="Slide 1"
          ></button>
          <button
            type="button"
            data-bs-target="#carouselExampleCaptions"
            data-bs-slide-to="1"
            aria-label="Slide 2"
          ></button>
          <button
            type="button"
            data-bs-target="#carouselExampleCaptions"
            data-bs-slide-to="2"
            aria-label="Slide 3"
          ></button>
        </div>
        <div class="carousel-inner">
          <div class="carousel-item active">
            <img
              src="https://cdn.asiatatler.com/asiatatler/i/hk/2021/06/01113317-most-watched-netflix-original-movies-2021-extraction_cover_1280x720.jpeg"
              class="d-block w-100"
              alt="..."
            />
            <div class="carousel-caption d-none d-md-block">
              <h5>Unlimited movies, TV shows.</h5>
              <p>Watch anywhere.</p>
            </div>
          </div>
          <div class="carousel-item">
            <img
              src="https://www.thedigitalfix.com/film/wp-content/uploads/sites/2/2020/01/zmobie.jpg"
              class="d-block w-100"
              alt="..."
            />
            <div class="carousel-caption d-none d-md-block">
              <h5>Unlimited movies, TV shows.</h5>
              <p>Watch anywhere.</p>
            </div>
          </div>
          <div class="carousel-item">
            <img
              src="https://s3-us-west-2.amazonaws.com/flx-editorial-wordpress/wp-content/uploads/2020/07/31175740/700TheIrishman.jpg"
              class="d-block w-100"
              alt="..."
            />
            <div class="carousel-caption d-none d-md-block">
              <h5>Unlimited movies, TV shows.</h5>
              <p>Watch anywhere.</p>
            </div>
          </div>
        </div>
        <button
          class="carousel-control-prev"
          type="button"
          data-bs-target="#carouselExampleCaptions"
          data-bs-slide="prev"
        >
          <span class="carousel-control-prev-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Previous</span>
        </button>
        <button
          class="carousel-control-next"
          type="button"
          data-bs-target="#carouselExampleCaptions"
          data-bs-slide="next"
        >
          <span class="carousel-control-next-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Next</span>
        </button>
      </div>
    </div>

    <form @submit.prevent="searchMovie()" class="search-box">
      <input
        type="text"
        placeholder="What are you looking for?"
        v-model="search"
      />
      <select v-model="selected" class="form-select">
        <option
          v-for="option in options"
          :key="option.id"
          :value="option.value"
        >
          {{ option.name }}
        </option>
      </select>
      <input type="submit" value="Search" />
    </form>

    <div class="movie-list">
      <div class="movie" v-for="movie in movies" :key="movie.id">
        <router-link :to="`/movie/${movie.id}/${selected}`" class="movie-link">
          <div class="product-image">
            <img :src="`${baseUrl}${movie.poster_path}`" alt="Movie Poster" />
            <div class="type">
              {{ movie.vote_average }}
            </div>
          </div>
          <template v-if="selected === 'movie'">
            <div class="image-detail">
              <p class="year">{{ movie.release_date }}</p>
              <h3>{{ movie.title }}</h3>
            </div>
          </template>
          <template v-else>
            <div class="image-detail">
              <p class="year">{{ movie.first_air_date }}</p>
              <h3>{{ movie.name }}</h3>
            </div>
          </template>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import axios from "axios";
// @ is an alias to /src
import env from "@/env.js";

export default {
  setup() {
    let selected = ref("movie");
    const search = ref("");
    const movies = ref([]);
    const baseUrl = ref("https://image.tmdb.org/t/p/w500");
    const options = ref([
      {
        id: 1,
        name: "Movie",
        value: "movie",
      },
      {
        id: 2,
        name: "TV Series",
        value: "tv",
      },
    ]);
    const searchMovie = () => {
      if (search.value !== "") {
        axios
          .get(
            `https://api.themoviedb.org/3/search/${selected.value}?api_key=${env.apikey}&query=${search.value}`
          )
          .then((response) => {
            movies.value = response.data.results;
            search.value = "";
            console.log(movies.value);
            console.log(selected);
          });
      }
    };
    return {
      search,
      movies,
      searchMovie,
      baseUrl,
      selected,
      options,
    };
  },
};
</script>

<style>
.home .feature-card {
  position: relative;
}

.home .featured-img {
  display: block;
  width: 100%;
  height: 300px;
  object-fit: cover;
  position: relative;
  z-index: -1;
}

.detail {
  position: absolute;
  right: 0;
  left: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.6);
  padding: 16px;
}

.detail h3 {
  color: #fff;
  margin-bottom: 16px;
}

.detail p {
  color: #fff;
}

.search-box {
  display: flex;
  flex-direction: column;
  /* justify-content: center; */
  align-items: center;
  padding: 16px;
}

.search-box input {
  display: block;
  /* appearance: none; */
  border: none;
  outline: none;
  background-color: none;
}

.search-box input[type="text"] {
  width: 100%;
  color: #fff;
  background-color: #496583;
  font-size: 20px;
  padding: 10px 16px;
  border-radius: 8px;
  margin-bottom: 15px;
  transition: 0.4s;
}

.search-box input[type="text"]::placeholder {
  color: #f3f3f3;
}

.search-box [type="submit"]:focus {
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
}

.search-box [type="submit"] {
  width: 100%;
  max-width: 300px;
  background-color: #42b883;
  padding: 16px;
  border-radius: 8px;
  color: #fff;
  font-size: 20px;
  text-transform: uppercase;
  transition: 0.3s;
  cursor: pointer;
}

.search-box [type="submit"]:hover {
  transform: translateY(-3px);
}

.search-box input[type="submit"]:active {
  background-color: #3b8070;
}

.search-box select {
  padding: 10px;
  width: 100%;
  max-width: 200px;
  margin-bottom: 15px;
  border-color: #fff;
  border-radius: 8px;
  outline: none;
  background-color: #f3f3f3;
}

.search-box select:focus {
  border-color: lightblue;
}

.movie-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0 8px;
}

.movie-list .movie {
  max-width: 50%;
  flex: 1 1 50%;
  padding: 16px 8px;
}

.movie-list .movie .movie-link {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.movie-list .movie .movie-link .product-image {
  position: relative;
  display: block;
}

.movie-list .movie .movie-link .product-image img {
  display: block;
  width: 100%;
  height: 275px;
  object-fit: cover;
}

.movie-list .movie .movie-link .product-image .type {
  position: absolute;
  padding: 8px 16px;
  background-color: #42b883;
  bottom: 16px;
  left: 0;
  text-transform: capitalize;
  color: #fff;
  font-weight: bold;
}

.movie-list .movie .movie-link .image-detail {
  background-color: #496583;
  padding: 16px 8px;
  flex: 1 1 100%;
  border-radius: 0 0 8px 8px;
}

.movie-list .movie .movie-link .image-detail .year {
  color: #aaa;
  font-size: 14px;
}

.movie-list .movie .movie-link .image-detail h3 {
  color: #fff;
  font-weight: 600;
  font-size: 18px;
}

.carousel-item > img {
  height: 550px;
}

.carousel-caption > h5 {
  font-size: 2rem;
}
.carousel-caption p {
  font-size: 1.5rem;
}
</style>
