<template>
  <div id="searchresultlist">
    <h1>
      <span class="badge badge-success">SearchResultList</span>
    </h1>

    <!-- SEARCH RESULT CARDS -->
    <div
      v-for="result in this.$props.results"
      v-bind:key="result.name">
        <search-result-list-card
          v-bind:theResult="result" />
    </div>
    <!-- <search-result-list-card
      class="row"
      v-for="result in this.$props.results"
      v-bind:key="result.id"
      v-bind:theResult="result" /> -->

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
    results: Array
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