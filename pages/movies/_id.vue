<template>
  <div>
    <div>
      <h1 class="text-3xl font-bold">{{ movieInfo.title }}</h1>
      <p class="w-1/2 mb-10">{{ movieInfo.overview }}</p>
      <img
        class="rounded-3xl"
        :src="`https://image.tmdb.org/t/p/w500/${movieInfo.poster_path}`"
        alt=""
      />
      <ul>
        <li v-for="(genre, index) in movieInfo.genres" :key="index"> {{ genre.name }}</li>
      </ul>
      <a :href="movieInfo.homepage" target="_blank">Watch Movie</a>
      <p class="review">{{ movieInfo.vote_average }}</p>
    </div>
      <pre>{{ movieInfo }}</pre>
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

background-position: right -200px top;
    background-size: cover;
    background-repeat: no-repeat;
    background-image: url(/t/p/w1920_and_h800_multi_faces/5hoS3nEkGGXUfmnu39yw1k52JX5.jpg);
