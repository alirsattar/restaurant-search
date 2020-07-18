<template>
  <div id="searchresultlist" class="row">
    <h1>
      <span class="badge badge-success">SearchResultList</span>
    </h1>

    <!-- SEARCH RESULT CARDS -->

    <!-- PAGINATOR -->
    <search-result-list-paginator></search-result-list-paginator>
  </div>
</template>

<script>
import SearchResultListPaginator from "./SearchResultListPaginator/SearchResultListPaginator";

export default {
  name: "SearchResultList",
  props: {},
  components: {
    SearchResultListPaginator
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