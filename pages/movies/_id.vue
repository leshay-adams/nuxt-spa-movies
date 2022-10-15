<template>
  <div class="">
    <h1 class="text-3xl font-bold">{{movieInfo.title}}</h1>
    <p class="w-1/2 mb-10">{{movieInfo.overview}}</p>
    <pre>{{movieInfo}}</pre>
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
