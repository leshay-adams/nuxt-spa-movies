<template>
  <div class="px-32 py-20">
    <NuxtLink class="button" :to="{ name: 'index' }"> 
    <button class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-2 px-4 rounded mb-6">BACK</button> </NuxtLink>
    <div class="flex gap-6">
      <div>
        <img
          class="max-w-none rounded-3xl"
          :src="`https://image.tmdb.org/t/p/w500/${movieInfo.poster_path}`"
          alt=""
        />
      </div>
      <div class="text-white w-1/2">
        <h1 class="text-3xl font-bold mb-4">{{ movieInfo.title }}</h1>
        <h1 v-if="movieInfo.tagline" class="text-xl text-yellow-400 italic mb-4">
          "{{ movieInfo.tagline }}"
        </h1>
        <p class="text-xl mb-10">{{ movieInfo.overview }}</p>
        <a v-if="movieInfo.homepage" :href="movieInfo.homepage" target="_blank"
          ><img class="w-32 cursor-pointer" src="~/assets/play.webp" alt=""
        /></a>
        <p class="underline mt-10 mb-2">Genres</p>
        <ul class="flex">
          <li
            class="text-yellow-500 genre"
            v-for="(genre, index) in movieInfo.genres"
            :key="index"
          >
            {{ genre.name }} &nbsp;
          </li>
        </ul>
        <p class="underline pt-4 pb-2">Released</p>
        <p>
          {{
            new Date(movieInfo.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </p>
        <p class="underline pt-4 pb-2">Runtime</p>
        {{ movieInfo.runtime }} minutes
        <p class="underline pt-4 pb-2">Languages</p>
        <p v-for="lang in movieInfo.spoken_languages">
          {{ lang.english_name }}
        </p>
      </div>
      <!-- <pre class="text-white">{{ movieInfo }}</pre> -->
    </div>
  </div>
</template>

<script>
export default {
  name: 'Movie',
  data() {
    return {
      movieInfo: {},
      id: this.$route.params.id,
    }
  },
  async fetch() {
    const data = await fetch(
      `https://api.themoviedb.org/3/movie/${this.id}?api_key=fb2707fe157f2dbd5108f4a0d77f10d7&language=en-US`
    ).then((res) => res.json())
    this.movieInfo = data
  },
}
</script>
<style>
body {
  background-color: #18181b;
}
.genre ~ .genre::before {
  content: ' | ';
}
</style>
