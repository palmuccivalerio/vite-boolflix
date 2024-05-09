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
      axios.get("https://api.themoviedb.org/3/search/movie", {
        params: {
          query: this.store.searched,
          api_key: this.store.apiKey,
        },
      }).then((resp) => {
        console.log(resp.data.results[0].original_title);
        console.log(resp.data.results[0].title);
        this.store.filmArr= resp.data.results

      });


    },
    getImgUrl(name){
      return new URL (`./assets/img/${name}.png`,import.meta.url).href
    },
    functionFlag(lang){
      let inArray = false 
      this.store.flag.forEach(curLang => {
        if (curLang === lang ){
          inArray = true
          console.log(curLang,lang);
        }
        console.log(inArray);
      });
      return inArray
    }
  },
};
</script>

<template>
  <AppSearch @nameTitle="search" />
  <ul>
    <li v-for="curFilm in this.store.filmArr">
      {{curFilm.title}} 
      {{curFilm.original_title}} 
      {{curFilm.vote_average}} 
      <img v-if="functionFlag(curFilm.original_language)" :src="getImgUrl(curFilm.original_language)" alt="">
      <p v-else> {{curFilm.original_language}}</p>
    </li>
    
  </ul>
</template>

<style></style>
