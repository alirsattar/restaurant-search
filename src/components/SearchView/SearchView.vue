<template>
  <div id="searchview">
    <div id="search-container" class="container-fluid">
      <div class="row">
        <!-- SEARCH INPUT BOX -->
        <div class="col-12 pr-0">
          <search-input-box
            @gotSearchQuery="onSearchQuery" />
        </div>
      </div>

      <!-- RESULTS FILTERS -->

      <!-- SORRY, DID NOT HAVE TIME TO COME BACK AND STYLE THESE PROPERLY ! -->
      <div class="row">
        <div class="col-12">
          <div
            id="resultfilters"
            class="">
            <select id="rating">
              <option value="rating" disabled selected>Rating</option>
            </select>
            <select id="price">
              <option value="" disabled selected>Price</option>
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
            </select>
            <select id="distance">
              <option value="" disabled selected>Distance</option>
              <option value="10">10 miles</option>
              <option value="25">20 miles</option>
              <option value="50">50 miles</option>
            </select>
          </div>

          <div
            id="resultcount"
            class="float-right"
            v-show="this.$data.totalEntries">
              <h5>{{ this.$data.totalEntries }} Results</h5>
          </div>
        </div>
      </div>
    
      <!-- RESULT CARDS CONTAINER -->
      <div class="row">
        <div class="col-sm-12 col-md-6 col-lg-8">

          <!-- DISPLAY MESSAGE ONLY IF NOT SEARCH RESULTS -->
          <template
            v-if="!this.$data.searchResults.length">
              <h2 class="mt-3">Search above by restaurant name, city, state, or Zip code</h2>
          </template>

          <!-- SEARCH RESULT LIST CONTAINER -->
          <search-result-list
            v-bind:results="this.$data.searchResults"
            v-bind:totalResults="this.$data.totalEntries"
            v-bind:perPage="this.$data.perPage"
            v-bind:currentPage="this.$data.currentPage"
            @goToPage="goToPage" />
        </div>

        <div class="col-sm-12 col-md-6 col-lg-4">
          <!-- SEARCH RESULT MAP -->
          <search-result-map
            v-bind:locations="this.$data.searchResults" />
        </div>
      </div>

    </div>
  </div>
</template>

<script>
const axios = require('axios');

import SearchInputBox from "./SearchInputBox/SearchInputBox.vue";
import SearchResultList from "./SearchResults/SearchResultList/SearchResultList";
import SearchResultMap from "./SearchResults/SearchResultMap/SearchResultMap";

export default {
  name: "SearchView",
  data: ()=> {
    return {
      searchResults: [],
      perPage: 0,
      currentPage: 0,
      totalEntries: 0,
      currentQuery: ''
    }
  },
  components: {
    SearchInputBox,
    SearchResultList,
    SearchResultMap
  },
  methods: {
    onSearchResults: function (searchResults) {
      this.$data.searchResults = searchResults;
    },
    onSearchQuery: function (query, pageNum) {
      const url = query + ( pageNum ? `&page=${pageNum}` : '' );

      this.makeApiCall(url)
        .then((apiResponse) => {
          const responseData = apiResponse.data;

          this.$data.searchResults = responseData.restaurants;
          this.$data.totalEntries = responseData.total_entries;
          this.$data.currentPage = responseData.current_page;
          this.$data.perPage = responseData.per_page;

          this.$data.currentQuery = query;
        })
        .catch((error)=> {
          console.error(error);
          this.$data.errorMessage = error;
        });
    },
    makeApiCall: function (searchUrl) {
      return axios.get(searchUrl);
    },
    goToPage: function(pageNum) {
      this.onSearchQuery(this.$data.currentQuery, pageNum);
    }
  }
};
</script>

<style scoped>
  #searchview {
    padding: 0px;
    margin: 0px;
  }

  #search-container {
    padding: 0vw 10vw;
  }
</style>
