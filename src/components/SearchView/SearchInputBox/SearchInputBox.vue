<template>
  <div id="searchinputbox">
    <h1>
      <span class="badge badge-success">SearchInputBox</span>
    </h1>

    <form>
      <input
        type="text"
        id="search-string"
        placeholder="Restaurant Name"
        v-model="searchString" />
      <button
        type="submit"
        class="btn btn-success ml-3"
        v-on:click="onSearch($event)">Search</button>
      <br><small id="error">{{ this.$data.errorMessage }}</small>
    </form>

    <div
      id="resultcount"
      v-show="this.$data.resultCount">
      {{ this.$data.resultCount }}
    </div>
    
  </div>
</template>

<script>
const axios = require("axios");

export default {
  name: "SearchInputBox",
  props: {},
  data: () => {
    return {
      searchApiBaseUrl: "http://opentable.herokuapp.com/api/restaurants?name=",
      searchString: "",
      errorMessage: '',
      resultCount: 0
    };
  },
  methods: {
    onSearch: function(buttonEvent) {
      buttonEvent.preventDefault();

      const urlWithQuery = this.$data.searchApiBaseUrl + this.$data.searchString;

      console.log({ urlWithQuery });

      axios.get(urlWithQuery)
        .then((searchResults) => {
          const restaurantResults = searchResults.data.restaurants;
          const totalResults = searchResults.data.total_entries;

          console.log({ restaurantResults, totalResults });

          this.$data.resultCount = totalResults;
          this.$emit('gotSearchResults', restaurantResults);
        })
        .catch((error)=> {
          console.error(error);
          this.$data.errorMessage = error;
        });
    }
  }
};
</script>

<style scoped>
#searchinputbox {
  border: 1px solid;
  padding: 10px;
  margin: 10px;
}

#error {
  color: red;
}
</style>