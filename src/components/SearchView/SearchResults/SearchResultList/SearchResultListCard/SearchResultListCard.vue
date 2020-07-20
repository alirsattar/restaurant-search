<template>
  <div class="searchresultlistcard col-12">
    <h4>
      <span class="badge badge-success">SearchResultListCard</span>
    </h4>
    <br>Image: <img class="restaurant-image" :src="theResult.image_url" alt="">
    <br>Name: {{ theResult.name }}
    <br>Distance:
    <br>Address: {{ theResult.address }}
    <br>Area: {{ theResult.area }}
    <br>City: {{ theResult.city }}
    <br>State: {{ theResult.state }}
    <br>Zip: {{ theResult.postal_code }}
    <br>Price: {{ theResult.price }}
    <br>Stars: {{  }}
    <br>Reservation Number: {{ theResult.phone }}
    <br>Website: {{ theResult.reserve_url }}
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

.restaurant-image {
  max-height: 100px;
}
</style>