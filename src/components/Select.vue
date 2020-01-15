<template>
  <div>
    <h1>Hello Select</h1>
    <select v-if="populated">
      <option v-for="genre in genres" :key="genre.id">
        {{ genre.name }}
      </option></select
    >
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "selector",
  data() {
    return {
      genres: [],
      populated: false
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
