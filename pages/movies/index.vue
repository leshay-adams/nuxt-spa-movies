<template>
    <div class="p-20 text-white">
      <p v-if="$fetchState.pending">Fetching movies...</p>
      <p v-else-if="$fetchState.error">An error occurred :(</p>
      <div v-else>
        <h1
          class="mb-12 text-3xl font-extrabold text-white md:text-5xl lg:text-6xl"
        >
          <span
            class="text-transparent bg-clip-text bg-gradient-to-r to-emerald-600 from-sky-400"
            >Zinc Movies.</span
          >
          It's Showtime.
        </h1>
        <div class="mb-20">
          <p class="text-lg font-normal lg:text-xl text-zinc-400 pb-4">
            Search All Movies
          </p>
  
          <div class="relative">
            <div
              class="flex absolute inset-y-0 left-0 items-center pl-3 pointer-events-none"
            >
            <img 
                class="w-5 h-5" @click="$fetch" src="~/assets/search-icon.svg" alt="">
            </div>
            <input
              class="block p-4 pl-10 w-1/2 text-sm text-zinc-900 bg-zinc-50 rounded-lg border border-zinc-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-zinc-700 dark:border-zinc-600 dark:placeholder-zinc-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
              type="text"
              @keyup.enter="$fetch"
              placeholder="eg. Prey"
              v-model="search"
            />
          </div>
        </div>
        <div class="flex mb-6 items-center">
          <h1 class="text-2xl font-bold text-zinc-100 mr-4">Popular Films</h1>
          <img class="w-6 cursor-pointer" @click="$fetch" src="~/assets/arrows-rotate-solid.svg" alt="">
        </div>
        <div class="grid grid-cols-4 gap-8">
          <div class="relative" v-for="(movie, index) in movies" :key="index">
            <NuxtLink :to="{ name: 'movies-id', params: { id: movie.id } }">
              <img
                class="rounded-3xl"
                :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                alt=""
              />
              <p
                class="bg-yellow-500 p-4 max-w-fit absolute top-0 right-0 rounded-bl-xl font-semibold"
              >
                {{ movie.vote_average }}
              </p>
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Movies',
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
  }
  </script>
  