<template>
  <div id="searchresultlist">    
    <!-- SEARCH RESULT CARDS -->
    <div id="results-cards-container">
      <div
        v-for="result in this.$props.results"
        v-bind:key="result.name">
          <search-result-list-card
            v-bind:theResult="result" />
      </div>
    </div>

    <!-- PAGINATOR -->
    <search-result-list-paginator
      v-if="this.$props.results.length"
      v-bind:totalResults="this.$props.totalResults"
      v-bind:perPage="this.$props.perPage"
      v-bind:currentPage="this.$props.currentPage"
      @goToPage="goToPage" />
  </div>
</template>

<script>
  import SearchResultListPaginator from "./SearchResultListPaginator/SearchResultListPaginator";
  import SearchResultListCard from './SearchResultListCard/SearchResultListCard';

  export default {
    name: "SearchResultList",
    props: {
      results:      Array,
      totalResults: Number,
      perPage:      Number,
      currentPage:  Number
    },
    components: {
      SearchResultListPaginator,
      SearchResultListCard
    },
    methods: {
      goToPage: function(pageNum) {
        this.$emit('goToPage', pageNum);
      }
    }
  };
</script>

<style scoped>
  @media screen and (min-width: 500px){
    #results-cards-container {
      height: 55vh;
      min-height: 400px;
      overflow: auto;
      -ms-overflow-style: none;  /* IE and Edge */
      scrollbar-width: none;  /* Firefox */
    }
  }

  #results-cards-container::-webkit-scrollbar {
    display: none;
  }
</style>