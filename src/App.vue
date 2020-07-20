<template>
  <div id="app" >
    <!-- OVERALL PAGE CONTAINER -->
    <div class="container-fluid">
      <!-- GENERAL APP HEADER -->
      <app-header />

      <!-- SEARCH VIEW -->
      <search-view msg="TESTING" />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

import AppHeader from "./components/AppHeader/AppHeader.vue";
import SearchView from "./components/SearchView/SearchView.vue";

@Component({
  components: {
    AppHeader,
    SearchView
  }
})
export default class App extends Vue {}
</script>

<style>
body {
  background-color: #f7f7f7;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  border: 1px solid;
  padding: 10px;
  margin-top: 0px;
}
</style>
