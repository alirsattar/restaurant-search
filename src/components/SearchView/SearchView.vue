<template>
  <div id="searchview" class="container-fluid">
    <div id="search-container">
      
      <!-- SEARCH INPUT BOX -->
      <div id="searchbox-input-container" class="row">
        <div class="col-12">
          <search-input-box
            @gotSearchQuery="onSearchQuery" />
        </div>
      </div>

      <!-- RESULTS FILTERS -->

      <div class="row">
        <div class="col-sm-12 col-lg-4">
          <div
            v-show="this.$data.totalEntries"
            id="searchfilters"
            class="row">

            <!-- RATING DROPDOWN -->
            <div
              id="rating-filter"
              class="filter col-sm-12 col-md-6 col-lg-4 col-xl-3 mb-2">
                <div class="dropdown">
                  <button
                    class="btn btn-light dropdown-toggle filter-dropdown"
                    type="button"
                    id="ratingdropdown"
                    data-toggle="dropdown"
                    aria-haspopup="true"
                    aria-expanded="false">
                      Rating
                  </button>
                  <div class="dropdown-menu" aria-labelledby="dropdownMenu2">
                    <button class="dropdown-item" type="button">(No rating info coming from API, sorry!)</button>
                  </div>
                </div>
            </div>

            <!-- PRICE DROPDOWN -->
            <div
              id="price-filter"
              class="filter col-sm-12 col-md-6 col-lg-4 col-xl-3 mb-2">
                <div class="dropdown">
                  <button
                    class="btn btn-light dropdown-toggle filter-dropdown"
                    type="button"
                    id="pricedropdown"
                    data-toggle="dropdown"
                    aria-haspopup="true"
                    aria-expanded="false">
                      Price
                  </button>
                  <div class="dropdown-menu" aria-labelledby="dropdownMenu2">
                    <button class="dropdown-item" type="button">
                      <i class="fas fa-dollar-sign"></i>
                    </button>
                    <button class="dropdown-item" type="button">
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                    </button>
                    <button class="dropdown-item" type="button">
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                    </button>
                    <button class="dropdown-item" type="button">
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                    </button>
                    <button class="dropdown-item" type="button">
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                      <i class="fas fa-dollar-sign"></i>
                    </button>
                  </div>
                </div>
            </div>

            <!-- DISTANCE DROPDOWN -->
            <div
              id="distance-filter"
              class="filter col-sm-12 col-md-6 col-lg-4 col-xl-3 mb-2">
                <div class="dropdown">
                  <button
                    class="btn btn-light dropdown-toggle filter-dropdown"
                    type="button"
                    id="distancedropdown"
                    data-toggle="dropdown"
                    aria-haspopup="true"
                    aria-expanded="false">
                      Distance
                  </button>
                  <div class="dropdown-menu" aria-labelledby="dropdownMenu2">
                    <button class="dropdown-item" type="button">10 miles</button>
                    <button class="dropdown-item" type="button">20 miles</button>
                    <button class="dropdown-item" type="button">50 miles</button>
                  </div>
                </div>
            </div>

          </div>
        </div>

        <!-- RESULTS COUNT -->
        <div class="col-sm-12 col-lg-8">
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
    /* padding: 0px;
    margin: 0px; */
  }

  #search-container {
    padding: 0vw 10vw;
  }

  .filter-dropdown {
    background-color: white;
    border: 2px #0d8fb3 solid;
    width: 100%;
    min-width: fit-content;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
</style>
