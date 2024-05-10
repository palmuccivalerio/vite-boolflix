<script>
import AppSearch from "./components/AppSearch.vue";
import { store } from "./store.js";
import axios from "axios";

export default {
  data() {
    return {
      store,
    };
  },

  components: {
    AppSearch,
  },

  methods: {
    search() {
      axios
        .get("https://api.themoviedb.org/3/search/movie", {
          params: {
            query: this.store.searched,
            api_key: this.store.apiKey,
          },
        })
        .then((resp) => {
          this.store.filmArr = resp.data.results;
        });
      axios
        .get("https://api.themoviedb.org/3/search/tv", {
          params: {
            query: this.store.searched,
            api_key: this.store.apiKey,
          },
        })
        .then((resp) => {
          this.store.tvArr = resp.data.results;
          console.log(this.store.tvArr)
        });
    },
    getImgUrl(name) {
      return new URL(`./assets/img/${name}.png`, import.meta.url).href;
    },
    functionFlag(lang) {
      let inArray = false;
      this.store.flag.forEach((curLang) => {
        if (curLang === lang) {
          inArray = true;
        }
      });
      return inArray;
    },
  },
};
</script>

<template>
  <AppSearch @nameTitle="search" />
  <h2>FILM</h2>
  <ul>
    <li v-for="curFilm in this.store.filmArr">
      {{ curFilm.title }}
      {{ curFilm.original_title }}
      {{ curFilm.vote_average }}
      <span v-for="x in 5">s</span>
      <img :src="'http://image.tmdb.org/t/p/w500/' + curFilm.poster_path" alt="">

      <img
        v-if="functionFlag(curFilm.original_language)"
        :src="getImgUrl(curFilm.original_language)"
        alt=""
      />
      <p v-else>{{ curFilm.original_language }}</p>
    </li>
  </ul>
  <H2>SERIE TV</H2>
  <ul>
    <li v-for= " curSeries in this.store.tvArr">
      {{curSeries.name}}
      {{curSeries.original_name}}    
      {{curSeries.vote_average}}
      <img :src="'http://image.tmdb.org/t/p/w500/' + curSeries.poster_path" alt="">
      <img
        v-if="functionFlag(curSeries.original_language)"
        :src="getImgUrl(curSeries.original_language)"
        alt=""
      />
      <p v-else>{{ curSeries.original_language }}</p>
  
    </li>
  </ul>
</template>

<style></style>
