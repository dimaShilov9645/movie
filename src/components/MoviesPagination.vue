<template>
  <div class="movies-pagination d-flex justify-content-center">
    <BPagination
      v-model="currentPageModel"
      :per-page="perPage"
      :total-rows="total"
      prev-text="Prev"
      next-text="next"
    />
  </div>
</template>

<script>
export default {
  name: "MoviesPagination",
  props: {
    currentPage: {
      type: Number,
      default: 1
    },
    total: {
      type: Number,
      default: 1
    },
    perPage: {
      type: Number,
      default: 1
    }
  },
  computed: {
    currentPageModel: {
      get() {
        return this.currentPage;
      },
      set(value) {
        this.$emit("pageChanged", value);
      }
    }
  }
};
</script>

<style scoped>
.movies-pagination {
  margin-top: auto;
}

.movies-pagination >>> .pagination .page-item .page-link {
  background-color: transparent;
  font-size: 12px;
  color: #ffffff;
  box-shadow: none;
}

.movies-pagination >>> .pagination .page-item.active .page-link {
  border-color: #ffffff;
  background-color: #ffffff;
  color: #000;
}

.movies-pagination >>> .pagination .page-item.disabled .page-link {
  border-color: lightgray;
}
</style>
