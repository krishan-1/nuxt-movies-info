<template>
  <div>
    <Loading v-if="$fetchState.pending" />
    <div v-else class="theme-container single-movie">
      <nuxt-link class="btn" :to="{ name: 'index' }">Back</nuxt-link>
      <div class="movie-info">
        <div class="movie-img">
          <img
            class="movie-poster"
            :src="`https://image.tmdb.org/t/p//w500/${movie.poster_path}`"
            alt="movie-image"
          />
        </div>

        <div class="movie-content">
          <h1>Title: {{ movie.title }}</h1>
          <p class="movie-fact tagline">
            <span>Tagline:</span> "{{ movie.tagline }}"
          </p>
          <p class="movie-fact">
            <span> Released:</span>
            {{
              new Date(movie.release_date).toLocaleString("en-us", {
                month: "long",
                day: "numeric",
                year: "numeric",
              })
            }}
          </p>
          <p class="movie-fact">
            <span>Duration:</span> {{ movie.runtime }} minutes
          </p>
          <p class="movie-fact">
            <span>Revenue:</span>
            {{
              movie.revenue.toLocaleString("en-us", {
                style: "currency",
                currency: "USD",
              })
            }}
          </p>
          <p class="movie-fact"><span>Overview:</span> {{ movie.overview }}</p>
          <div class="vid-link">
            <iframe
              :src="ytVid"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "single-movie",
  head() {
    return {
      title: this.movie.title,
    };
  },
  data() {
    return {
      movie: "",
      getVid: "",
    };
  },

  async fetch() {
    await this.getSingleMovie();
    await this.getVideo();
  },
  // async fetch() {
  //   await this.getVideo();
  // },

  computed: {
    ytVid() {
      // return "https://www.youtube.com/embed/" + this.getVid?.results[4]?.key;

      let storeArrKey = this.getVid.results
        .map((x) => (x.type === "Trailer" ? x.key : null))
        .filter((x) => x !== null);

      return `https://www.youtube.com/embed/${storeArrKey[0]}`;
    },
  },

  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`
      );
      const result = await data;
      this.movie = result.data;
    },

    async getVideo() {
      const data = axios.get(`
        https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/videos?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`);
      const result = await data;
      this.getVid = result.data;
      console.log(this.getVid);
    },
  },
};
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .btn {
    align-self: flex-start;
    margin-bottom: 32px;
  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;

    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }

    .movie-img {
      .movie-poster {
        max-height: 500px;
        width: 100%;

        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }

    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;

        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }

      .tagline {
        font-style: italic;

        span {
          font-style: normal;
        }
      }

      .vid-link {
        iframe {
          aspect-ratio: 16/9;
          width: 720px;
        }
      }
    }
  }
}
</style>
