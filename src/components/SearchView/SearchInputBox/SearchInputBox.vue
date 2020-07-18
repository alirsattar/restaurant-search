<template>
  <div id="searchinputbox">
    <h1>
      <span class="badge badge-success">SearchInputBox TESTING</span>
    </h1>

    <input
      type="text"
      id="search-string"
      v-model="searchString" />
    <button
      class="btn btn-success ml-3"
      v-on:click="search()">Search</button>
    <br><small id="error">{{ this.$data.errorMessage }}</small>

    <br>Results:
    <ul
      v-for="city in this.$data.searchResults"
      v-bind:key="city">
        <li>{{ city }}</li>
    </ul>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  name: "SearchInputBox",
  props: {},
  data: () => {
    return {
      searchApiBaseUrl: "http://opentable.herokuapp.com/api/",
      searchString: "",
      errorMessage: '',
      searchResults: []
    };
  },
  components: {},
  methods: {
    search() {
      let results;
      const urlWithQuery = this.$data.searchApiBaseUrl + this.$data.searchString;

      console.log({ urlWithQuery });

      axios.get(urlWithQuery)
        .then((searchResults) => {
          console.log(searchResults.data);
          results = searchResults.data.cities;
          this.$data.searchResults = searchResults.data.cities
        })
        .catch((error)=> {
          console.error(error);
          this.$data.errorMessage = error;
        });

      return results;
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