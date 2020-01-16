<template>
  <div id="main-wrap">
    <div id="selector">
      <select v-model="selected">
        <option v-for="genre in genres" :key="genre.id" v-bind:value="genre.id">
          {{ genre.name }}
        </option>
      </select>
      <button v-on:click="getResult">Find a movie!</button>
    </div>
    <div id="moviecard" v-if="search">
      <h1>{{ randResult.original_title }}</h1>
      <img
        :src="'https://image.tmdb.org/t/p/w500' + randResult.poster_path"
        alt="poster"
      />
      <p>{{ randResult.overview }}</p>
      <button
        v-on:click="
          addToWatchlist(
            randResult.original_title,
            randResult.id,
            randResult.poster_path
          )
        "
      >
        Add this to your Watchlist
      </button>
    </div>
  </div>
</template>
<style scoped>
#selector {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
#moviecard {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: auto;
  margin-right: 2em;
  margin-left: 2em;
}
#moviecard h1 {
  grid-column-start: 2;
  grid-column-end: 6;
  grid-row: 1;
}
#moviecard img {
  grid-row-start: 2;
  grid-row-end: 4;
  grid-column-start: 1;
  grid-column-end: 4;
  max-width: 90%;
}
#moviecard p {
  text-align: left;
  grid-column-start: 4;
  grid-column-end: 7;
}
#moviecard button {
  grid-column-start: 4;
  grid-column-end: 6;
  height: 50%;
}
</style>
<script>
import axios from "axios";
export default {
  name: "selector",
  data() {
    return {
      genres: [],
      populated: false,
      search: false,
      selected: "28",
      randResult: ""
    };
  },
  created() {
    axios
      .get(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=62a61cc56aa5267930dded44f273f098&language=en-US"
      )
      .then(res => (this.genres = res.data.genres))
      .then((this.populated = true))
      .then(window.console.log(this.genres));
  },
  methods: {
    getResult: function getResult() {
      this.search = true;
      const randNumber = Math.floor(Math.random() * 20);
      axios
        .get(
          "https://api.themoviedb.org/3/discover/movie?api_key=62a61cc56aa5267930dded44f273f098&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&with_genres=" +
            this.selected
        )
        .then(res => (this.randResult = res.data.results[randNumber]));
    },
    addToWatchlist: function addToWatchlist(name, id, poster_path) {
      axios
        .post("https://wtwt-back.herokuapp.com/watchlist", {
          name: name,
          id: id,
          poster_path: poster_path
        })
        .catch(err => window.console.log(err));
    }
  }
};
</script>
