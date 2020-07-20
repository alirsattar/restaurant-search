<template>
  <div class="searchresultlistcard col-12">
    <div class="row">
      <div class="col-5">
        <div class="row">
          <div class="col-4">
            <img class="restaurant-image img-fluid" src="images/hungrytime_icon.jpg" alt="">
          </div>
          <div class="col-8">
            {{ theResult.name }}
            <br>Distance: | {{ theResult.address }}
            <br>Price: {{ theResult.price }}
          </div>
        </div>
      </div>
      <div class="col-2">
      </div>
      <div
        id="actions"
        class="col-5 ">
          <span class="">Make a Reservation</span>
          <br><button class="reservation-button btn btn-info "> {{ formatPhone(theResult.phone) }}</button>
          <br><a :href="theResult.reserve_url" target="_blank">See website</a> | <i class="fa fa-heart"></i> Save
      </div>
    </div>
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
  methods: {
    formatPhone: (numberString)=> {
      const cleaned = ('' + numberString).replace(/\D/g, '')
      const match = cleaned.match(/^(\d{3})(\d{3})(\d{4})$/)
      if (match) {
        return '(' + match[1] + ') ' + match[2] + '-' + match[3]
      }
      return null;
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
    min-width: 100%;
  }

  .reservation-button {
    width: 100%;
  }

  #actions {
    text-align: center;
  }
</style>