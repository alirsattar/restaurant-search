<template>
  <div id="searchview">
    <div id="search-container" class="container-fluid">
      <div class="row">
        <!-- SEARCH INPUT BOX -->
        <div class="col-12">
          <search-input-box
            @gotSearchQuery="onSearchQuery" />
        </div>
      </div>

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
              {{ this.$data.totalEntries }} Results
          </div>
        </div>
      </div>
    
      <div class="row">
        <div class="col-8">
          <!-- SEARCH RESULT LIST CONTAINER -->
          <search-result-list
            v-bind:results="this.$data.searchResults"
            v-bind:totalResults="this.$data.totalEntries"
            v-bind:perPage="this.$data.perPage"
            v-bind:currentPage="this.$data.currentPage" />
        </div>

        <div class="col-4">
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
      totalEntries: 0
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
    onSearchQuery: function (query) {
      this.makeApiCall(query)
        .then((apiResponse) => {
          const responseData = apiResponse.data;

          console.log(responseData);

          this.$data.searchResults = responseData.restaurants;
          this.$data.totalEntries = responseData.total_entries;
          this.$data.currentPage = responseData.current_page;
          this.$data.perPage = responseData.per_page;
        })
        .catch((error)=> {
          console.error(error);
          this.$data.errorMessage = error;
        });
    },
    makeApiCall: function (searchUrl) {
      return axios.get(searchUrl);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #searchview {
    padding: 0px;
    margin: 0px;
  }
</style>
