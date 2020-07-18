<template>
  <div id="searchinputbox">
    <h1>
      <span class="badge badge-success">SearchInputBox</span>
    </h1>

    <input type="text" name="" id="search-string">
    &nbsp;
    &nbsp;
    <button class="btn btn-success">Search</button>
  </div>
</template>

<script>
export default {
  name: "SearchInputBox",
  props: {},
  components: {}
};
</script>

<style scoped>
  #searchinputbox {
    border: 1px solid;
    padding: 10px;
    margin: 10px;
  }
</style>