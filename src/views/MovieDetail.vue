<template>
  <template v-if="$route.params.type === 'movie'">
    <div class="movie-detail">
      <h2>{{ movie.title }}</h2>
      <p>{{ movie.release_date }}</p>
      <img :src="`${baseUrl}${movie.poster_path}`" alt="Movie Poster" />
      <div class="genre">
        <span v-for="genre in movie.genres" :key="genre.id">{{
          genre.name
        }}</span>
      </div>
      <p>{{ movie.overview }}</p>
    </div>
  </template>
  <template v-else>
    <div class="movie-detail">
      <h2>{{ movie.name }}</h2>
      <p>{{ movie.first_air_date }}</p>
      <img :src="`${baseUrl}${movie.poster_path}`" alt="Movie Poster" />
      <p>{{ movie.overview }}</p>
    </div>
  </template>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import axios from "axios";
import env from "@/env.js";

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();
    const baseUrl = ref("https://image.tmdb.org/t/p/w500");

    onBeforeMount(() => {
      axios
        .get(
          `https://api.themoviedb.org/3/${route.params.type}/${route.params.id}?api_key=${env.apikey}&language=en-US`
        )
        .then((response) => {
          movie.value = response.data;
          console.log(movie.value);
        });
    });

    return {
      movie,
      baseUrl,
    };
  },
};
</script>

<style>
.movie-detail {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 16px;
}

.movie-detail h2 {
  color: #fff;
  font-weight: 600;
  font-size: 28px;
  margin-bottom: 16px;
}

.movie-detail img {
  display: block;
  max-width: 300px;
  margin-bottom: 16px;
}

.movie-detail p {
  color: #fff;
  font-size: 18px;
  line-height: 1.4;
}

.movie-detail .genre {
  background-color: #eee;
  width: 300px;
  display: flex;
  justify-content: center;
  border-radius: 0 0 8px 8px;
  margin-top: -16px;
  margin-bottom: 10px;
}

.movie-detail span {
  padding: 14px;
  color: #aaa;
}

.movie-detail span:not(:last-child) {
  border-right: 1px solid #bbb;
}

@media (max-width: 375px) {
  .movie-detail h2 {
    font-size: 20px;
  }
  .movie-detail p {
    font-size: 15px;
  }
  .movie-detail img {
    max-width: 200px;
  }
}
@media (min-width: 768px) {
  .movie-detail h2 {
    font-size: 35px;
  }
  .movie-detail p {
    font-size: 23px;
    text-align: justify;
  }
  .movie-detail img {
    max-width: 400px;
  }
}

@media (min-width: 1000px) {
  .movie-detail h2 {
    font-size: 35px;
  }
  .movie-detail p {
    font-size: 18px;
    text-align: justify;
  }
  .movie-detail img {
    max-width: 300px;
  }
}
</style>
