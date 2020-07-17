<template>
  <div id="app">
    <!-- GENERAL APP HEADER -->
    <app-header/>

    <!-- SEARCH VIEW -->
    <search-view
      msg="TESTING"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

import AppHeader from './components/AppHeader/AppHeader.vue';
import SearchView from './components/SearchView/SearchView.vue';

@Component({
  components: {
    AppHeader,
    SearchView
  },
})
export default class App extends Vue {}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
