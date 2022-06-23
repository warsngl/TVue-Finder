<template lang='pug'>
.p-2.flex.flex-col.border-2.rounded-3xl.border-gray-400.shadow-lg.bg-green-200.cursor-pointer(
  class="hover:bg-purple-700 my-2 md:mx-2 w-auto 2xl:w-96 md:w-60 xl:w-80",
  @click="showDescription"
)
  span.text-red-400.font-bold.text-3xl.text-center.underline(
    @click.self="searchInGoogle"
  ) {{ movie.Title }}
  Transition(mode="out-in")
    .back.flex-1(v-if="isShowInfo")
      p.text-sm Genre: {{ info.Genre }}
      p.text-sm Actors: {{ info.Actors }}
      p(v-if="info.BoxOffice != 'N/A'") Earn: {{ info.BoxOffice }}
      p Description: {{ info.Plot }}
      p.text-sm Country: {{ info.Country }}
      p.text-sm(v-if="info.Runtime != 'N/A'") Duration: {{ info.Runtime }}
      p.text-sm {{ rating }}
    img.front.rounded-xl.my-auto.flex-1(v-else, :src="movie.Poster")
  .flex.justify-evenly
    span.text-blue-400 {{ movie.Type }}
    span {{ movie.Year }}
</template>

<script>
export default {
  props: ["movie"],
  data: () => ({
    info: {},
    isShowInfo: false,
  }),
  methods: {
    showDescription() {
      this.isShowInfo
        ? (this.isShowInfo = false)
        : this.$axios
            .get(
              `https://www.omdbapi.com/?t=${this.movie.Title}&apikey=c5cc3d12`
            )
            .then((res) => {
              this.info = res.data;
              this.isShowInfo = true;
            });
    },
    searchInGoogle() {
      window.open(
        `https://www.google.com/search?q=${this.movie.Title}`,
        "_blank"
      );
    },
  },
  computed: {
    rating() {
      let [i, r, m] = [
        this.info.Ratings[0]?.Value,
        this.info.Ratings[1]?.Value,
        this.info.Ratings[2]?.Value,
      ];
      console.log(i, r, m);
      let rating;
      return `${i ? "IMDb: " + i : ""} ${r ? "Rotten Tomatoes: " + r : ""} ${
        m ? "Metacritic: " + m : ""
      }`;
    },
  },
};
</script>

<style scoped>
img {
  min-width: 100%;
}

.v-enter-active {
  animation: faded 0.4s;
}
.v-leave-active {
  animation: faded 0.4s reverse;
}
/* Transition {
  perspective: 1000px;
}
.v-enter-active,
.v-leave-active {
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  transition: all 1s ease;
}
.v-enter-from,
.v-leave-to {
  transform: rotateY(180deg);
}
.back {
  transform: rotateY(180deg);
} */
@keyframes faded {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    opacity: 1;
  }
}
</style>