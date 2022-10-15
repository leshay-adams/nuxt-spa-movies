<template>
  <div>
    <p v-if="$fetchState.pending">Fetching movies...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>
    <div v-else>
      <label for="">SEARCH ALL FILMS</label>
      <input type="text" @keyup.enter="$fetch" placeholder="eg. Prey" v-model="search" />
      <h1>POPULAR FILMS</h1>
      <div class="grid grid-cols-4 gap-4">
        <div v-for="(movie, index) in movies" :key="index">
          <NuxtLink :to="{ name: 'movies-id', params: { id: movie.id } }">
            <h1>{{ movie.original_title }}</h1>
            <img
              class="rounded-3xl"
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
            />
          </NuxtLink>
        </div>
      </div>
      <button @click="$fetch">Refresh</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'index',
  data() {
    return {
      movies: [],
      search: '',
    }
  },
  async fetch() {
    if (this.search !== '') {
      await this.searchMovies()
    } else {
      await this.fetchMovies()
    }
  },
  methods: {
    async fetchMovies() {
      const data = await fetch(
        `https://api.themoviedb.org/3/movie/popular?api_key=fb2707fe157f2dbd5108f4a0d77f10d7&language=en-US&page=1`
      ).then((res) => res.json())

      this.movies = data.results
    },
    async searchMovies() {
      const data = await fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=fb2707fe157f2dbd5108f4a0d77f10d7&language=en-US&page=1&query=${this.search}`
      ).then((res) => res.json())
      this.movies = data.results
    },
  },
  // watch: {
  //   search(searchInput) {
  //     if (searchInput.length > 3) {
  //       setTimeout(this.$fetch(), 10000);
  //     }
  //   }
  // },
}
</script>
