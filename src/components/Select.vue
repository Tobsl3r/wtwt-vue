<template>
  <div id="main-wrap">
    <div id="selector">
      <select v-model="selected">
        <option v-for="genre in genres" :key="genre.id" v-bind:value="genre.id">
          {{ genre.name }}
        </option>
      </select>
      <button v-on:click="search = true">Find a movie!</button>
    </div>
    <div id="moviecard" v-if="search">
      <h1>Movie!</h1>
    </div>
  </div>
</template>
<style scoped>
#selector {
  display: flex;
  flex-direction: row;
  justify-content: center;
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
      selected: "28"
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
  beforeCreate() {
    window.console.log("I don't exist yet");
  }
};
</script>
