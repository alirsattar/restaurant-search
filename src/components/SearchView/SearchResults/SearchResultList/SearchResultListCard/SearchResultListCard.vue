<template>
  <div class="searchresultlistcard col-12">
    <h4>
      <span class="badge badge-success">SearchResultListCard</span>
    </h4>
    {{ theResult.name }}
  </div>
</template>

<script>
export default {
  name: "SearchResultListCard",
  props: {
    theResult: {
      type: Object,
      required: true
    }
  },
  mounted: function () {
    console.log( this.$props.theResult );
  }
};
</script>

<style>
.searchresultlistcard {
  max-width: -webkit-fill-available;
  border: 1px solid;
  padding: 10px;
  margin: 0px 10px 10px 10px;
}
</style>