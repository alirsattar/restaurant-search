<template>
  <div id="content">
    <h1>
      <span class="badge badge-success">SearchView</span>
    </h1>
    <h2>msg in SearchView: {{ msg }}</h2>

    <!-- SEARCH INPUT BOX -->
    <search-input-box></search-input-box>

    <!-- SEARCH RESULTS CONTAINER -->
  </div>
</template>

<script>
import SearchInputBox from './SearchInputBox/SearchInputBox.vue';

export default {
  name: "SearchView",
  props: {
    msg: String
  },
  components: {
    SearchInputBox
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
}
</style>
