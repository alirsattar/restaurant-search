<template>
  <div id="app">
    <!-- GENERAL APP HEADER -->
    <app-header/>

    <!-- SEARCH VIEW -->
    <search-page
      msg="TESTING 2"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

import AppHeader from './components/AppHeader/AppHeader.vue';
import SearchPage from './components/SearchPage.vue';

@Component({
  components: {
    AppHeader,
    SearchPage
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
