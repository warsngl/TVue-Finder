<template lang='pug'>
.w-full.flex.flex-col.justify-center(ref="scrollComponent")
  h1.flex.items-center.text-5xl.font-bold.mx-auto
    span TVue
    svg(
      viewBox="0 0 1280.000000 1228.000000",
      width="50.000000pt",
      height="50.000000pt"
    )
      g(
        transform="translate(0.000000,1228.000000) scale(0.100000,-0.100000)",
        fill="#4caf50",
        stroke="none"
      )
        path(
          d="M3580 10519 c-418 -47 -855 -233 -1197 -510 -191 -155 -415 -411 -552 -631 -303 -485 -468 -1037 -512 -1713 -76 -1171 375 -2000 1524 -2802 212 -148 408 -273 822 -523 570 -344 790 -490 1082 -717 659 -511 1296 -1213 1537 -1693 29 -58 58 -121 65 -140 l13 -35 18 50 c139 397 809 1167 1560 1795 329 275 617 476 1240 865 743 464 1044 683 1364 995 484 471 737 947 833 1567 24 155 24 661 0 863 -59 490 -166 864 -357 1240 -135 265 -301 491 -511 695 -369 358 -792 567 -1309 647 -133 20 -528 17 -665 -5 -787 -131 -1435 -583 -1911 -1332 -64 -100 -179 -313 -229 -426 l-39 -86 -101 201 c-55 111 -137 260 -182 332 -497 793 -1205 1275 -2008 1364 -105 12 -378 11 -485 -1z"
        )
    span Finder
  input.border-2.p-6.mt-4.mx-auto.rounded-3xl.shadow.text-3xl.text-indigo-600(
    class="focus:outline-none focus:ring-4 focus:ring-light-blue-600",
    @keyup.enter="(movies = []), searchMovieByName()",
    v-model="search",
    placeholder="Type to search movie..."
  )
  h1.text-red-200.text-6xl.mt-6.mx-auto(v-if="error") {{ error }}
  .movies-list.flex.flex-wrap.flex-start.justify-center(v-if="movies")
    MovieCard(v-for="movie in movies", :key="movie.imdbID", :movie="movie")
  TheLoading(v-if="isLoading")
</template>

<script>
export default {
  data: () => ({
    search: "",
    movies: [],
    isLoading: false,
    error: false,
    page: 1,
  }),
  methods: {
    searchMovieByName() {
      this.isLoading = true;
      this.$axios
        .get(
          `https://www.omdbapi.com/?s="${this.search}"&page=${this.page}&apikey=c5cc3d12`
        )
        .then((res) => {
          res.data.Response == "False"
            ? (this.error = res.data.Error)
            : ((this.movies = [...this.movies, ...res.data.Search]),
              (this.isLoading = false));
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });
    },
    handleScroll(e) {
      let element = this.$refs.scrollComponent;
      if (
        element.getBoundingClientRect().bottom < window.innerHeight + 10 &&
        !this.isLoading
      ) {
        console.log("nextpage");
        this.page++;
        this.searchMovieByName();
      }
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
