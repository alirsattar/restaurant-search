<template>
  <div id="searchresultlist">
    <h1>
      <span class="badge badge-success">SearchResultList</span>
    </h1>

    <!-- SEARCH RESULT CARDS -->
    TOTAL RESULTS: {{ this.$props.totalResults }} | PER PAGE: {{ this.$props.perPage }} | CURRENT PAGE: {{ this.$props.currentPage }}
    <div
      v-for="result in this.$props.results"
      v-bind:key="result.name">
        <search-result-list-card
          v-bind:theResult="result" />
    </div>

    <!-- PAGINATOR -->
    <search-result-list-paginator></search-result-list-paginator>
  </div>
</template>

<script>
import SearchResultListPaginator from "./SearchResultListPaginator/SearchResultListPaginator";
import SearchResultListCard from './SearchResultListCard/SearchResultListCard';

export default {
  name: "SearchResultList",
  props: {
    results: Array,
    totalResults: Number,
    perPage: Number,
    currentPage: Number
  },
  components: {
    SearchResultListPaginator,
    SearchResultListCard
  }
};
</script>

<style scoped>
#searchresultlist {
  border: 1px solid;
  padding: 10px;
  margin: 0px 10px 10px 10px;
}
</style>