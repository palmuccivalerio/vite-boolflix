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
        console.log(resp);
      });


    },
  },
};
</script>

<template>
  <AppSearch @nameTitle="search" />
</template>

<style></style>
