<template>
  <div id="searchview">
    <h1>
      <span class="badge badge-success">SearchView</span>
    </h1>
    <h2>msg in SearchView: {{ msg }}</h2>

    <div class="row">
      <!-- SEARCH INPUT BOX -->
      <div class="col-12">
        <search-input-box
          @gotSearchResults="onSearchResults" />
      </div>
    </div>

    <div class="row">
      <div class="col-8">
        <!-- SEARCH RESULT LIST CONTAINER -->
        <search-result-list
          v-bind:results="this.$data.searchResults" />
      </div>

      <div class="col-4">
        <!-- SEARCH RESULT MAP -->
        <search-result-map></search-result-map>
      </div>
    </div>
  </div>
</template>

<script>
import SearchInputBox from "./SearchInputBox/SearchInputBox.vue";
import SearchResultList from "./SearchResults/SearchResultList/SearchResultList";
import SearchResultMap from "./SearchResults/SearchResultMap/SearchResultMap";

export default {
  name: "SearchView",
  props: {
    msg: String
  },
  data: ()=> {
    return {
      searchResults: []
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
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#searchview {
  border: 1px solid;
  padding: 10px;
  margin: 10px;
}
/* 
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
} */
</style>
