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
          console.log(this.store.tvArr);
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
    starIcon(num) {
      console.log(Math.round(num / 2));
      return Math.round(num / 2);
    },
  },
};
</script>

<template>
  <AppSearch @nameTitle="search" />
   <div class="results-list">
    <h2 class="title">FILM</h2>
    <section class="movies">
      <div v-for="curFilm in this.store.filmArr" class="card">
        <div class="card-back">
          <h3>{{ curFilm.title }}</h3>
          <h6>{{ curFilm.original_title }}</h6>
          <img
            v-if="functionFlag(curFilm.original_language)"
            :src="getImgUrl(curFilm.original_language)"
            alt=""
          />
          <p v-else>{{ curFilm.original_language }}</p>
          <div class="vote">
            <i
              class="fas fa fa-solid fa-star"
              v-for="star in starIcon(curFilm.vote_average)"
            ></i>
            <i
              class="fa-regular fa-star"
              v-for="star in 5 - starIcon(curFilm.vote_average)"
            ></i>
          </div>
        </div>
        <div class="card-front">
          <img
            :src="`https://image.tmdb.org/t/p/w342/${curFilm.poster_path}`"
            alt="curFilm.title"
          />
        </div>
      </div>
    </section>

    <H2 class="title">SERIE TV</H2>

    <section class="movies">
      <div v-for="curSeries in this.store.tvArr" class="card">
        <div class="card-back">
          <h3>{{ curSeries.name }}</h3>
          <h6>{{ curSeries.original_name }}</h6>
          <img
            v-if="functionFlag(curSeries.original_language)"
            :src="getImgUrl(curSeries.original_language)"
            alt=""
          />
          <p v-else>{{ curSeries.original_language }}</p>
          <div class="vote">
            <i
              class="fas fa fa-solid fa-star"
              v-for="star in starIcon(curSeries.vote_average)"
            ></i>
            <i
              class="fa-regular fa-star"
              v-for="star in 5 - starIcon(curSeries.vote_average)"
            ></i>
          </div>
        </div>
        <div class="card-front">
          <img
            :src="`https://image.tmdb.org/t/p/w342/${curSeries.poster_path}`"
            alt="curSeries.title"
          />
        </div>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>
.results-list {
  padding: 20px;
img{
  max-width: 100%;
  max-height: 100%;
}

  .title{
    color: red;
    font-size:xx-large;
  }

  .movies,
  .series {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
  }

  .card {
    border: 20px;
    color: white;
    height: 450px;
    width: calc(100% / 5 - 20px);

    &:hover {
      .card-back {
        display: block;
      }

      .card-front {
        display: none;
      }
    }

    .card-back {
      display: none;
      padding: 20px;

      .flag {
        width: 10px;
      }

      .card-front {
        height: 300px;
        margin: 20px;
      }
    }
  }
}
</style>
