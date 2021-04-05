<template>
  <BContainer>
    <h3 class="list-title">{{ listTitle }}</h3>
    <BRow>
      <template v-if="isExist">
        <BCol v-for="(movie, key) in list" :key="key" cols="3">
          <MovieItem
            :movie="movie"
            @removeItem="onRemoveItem"
            @showModal="onShowMovieInfo"
            @mouseover.native="onMouseOver(movie.Poster)"
          />
        </BCol>
      </template>
      <template v-else>
        <div>Empty list</div>
      </template>
    </BRow>
    <BModal
      :id="movieInfoModalID"
      body-class="movie-modal-body"
      hide-footer
      hide-header
      size="xl"
    >
      <MovieInfoModalContent
        :movie="selectedMovie"
        @closeModal="onCloseModal"
      />
    </BModal>
  </BContainer>
</template>

<script>
import MovieItem from "@/components/MovieItem";
import { mapActions, mapGetters } from "vuex";
import MovieInfoModalContent from "@/components/MovieInfoModalContent";

export default {
  name: "MoviesList",
  components: { MovieInfoModalContent, MovieItem },
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  data: () => ({
    movieInfoModalID: "movie-info",
    selectedMovieID: ""
  }),
  computed: {
    ...mapGetters("movies", ["isSearch"]),
    isExist() {
      return Boolean(Object.keys(this.list).length);
    },
    listTitle() {
      return this.isSearch ? "Search Result" : "IMDB Top 250";
    },
    selectedMovie() {
      return this.selectedMovieID ? this.list[this.selectedMovieID] : null;
    }
  },
  methods: {
    ...mapActions(["showNotify"]),
    ...mapActions("movies", ["removeMovie"]),
    onMouseOver(poster) {
      this.$emit("changePoster", poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(
        `Are you sure delete ${title}?`
      );

      if (isConfirmed) {
        this.removeMovie(id);
        this.showNotify({
          msg: "Movie deleted successful",
          variant: "success",
          title: "Success"
        });
      }
    },
    onShowMovieInfo(id) {
      this.selectedMovieID = id;
      this.$bvModal.show(this.movieInfoModalID);
    },
    onCloseModal() {
      this.selectedMovieID = null;
      this.$bvModal.hide(this.movieInfoModalID);
    }
  }
};
</script>

<style scoped>
.list-title {
  position: relative;
  font-size: 50px;
  margin-bottom: 30px;
  color: #ffffff;
}
</style>

<style>
.movie-modal-body {
  padding: 0 !important;
}
</style>
