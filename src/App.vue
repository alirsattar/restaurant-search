<template>
  <div id="app" >
    <!-- OVERALL PAGE CONTAINER -->
    <div id="app-container" class="container-fluid">
      <!-- GENERAL APP HEADER -->
      <app-header />

      <!-- SEARCH VIEW -->
      <search-view />
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
  @font-face {
    font-family: 'NexaBold';
    src: local('fonts/Nexa-Bold.otf');
    font-weight: normal;
    font-style: normal;
  }

  body {
    background-color: #f7f7f7;
  }

  #app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin: 0px;
    padding: 0px;
    width: 100%;
  }

  #app-container {
    width: 100%;
    padding: 0px;
  }
</style>
