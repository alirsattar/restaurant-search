<template>
  <div class="searchresultlistcard col-12">
    <div class="row">

      <!-- RIGHT SIDE -->
      <div class="col-6">
        <div class="row">

          <!-- IMAGE CONTAINER -->
          <div class="col-2">
            <img class="restaurant-image img-fluid" src="images/hungrytime_icon.jpg" alt="">
          </div>

          <!-- RESTAURANT INFO -->
          <div class="col-10">
            <h4 class="text"><strong>{{ theResult.name }}</strong></h4>
            <h6 class="brown"><strong>Restaurant Type</strong></h6>
            <h5><i class="fas fa-map-marker-alt highlighted"></i> <span class="text">0.68 mi | {{ theResult.address }}</span></h5>
            <template
              v-for="index in 5">
                <h3
                  class="dollarsign mr-2"
                  v-bind:key="index">
                  <i
                  :class="{highlighted: isHighlighted(index)}"
                  class="fas fa-dollar-sign"></i>
                </h3>
            </template>
            {{ theResult.price }}
          </div>
        </div>
      </div>

      <!-- MIDDLE -->
      <div class="col-3">
        <img src="images/hungrytime_stars.png" alt="">
      </div>

      <!-- RIGHT SIDE -->
      <div
        id="actions"
        class="col-3">
          <h5 class="text">Make a Reservation</h5>
          <button class="reservation-button btn btn-info blue"><i class="fas fa-phone-alt mr-2"></i>{{ formatPhone(theResult.phone) }}</button>
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
    },
    isHighlighted: function(index) {
      return (index <= this.$props.theResult.price ? true : false);
    }
  }
};
</script>

<style scoped>
  .searchresultlistcard {
    min-height: fit-content;
    max-width: -webkit-fill-available;
    background-color: white;
    margin-bottom: 20px;
    padding: 20px;
    margin: 5px 10px 10px 10px;
    border-radius: 5px;
    box-shadow: 0px 1px 5px #888888;
  }

  .restaurant-image {
    /* min-width: 100%; */
    /* max-height: 50%; */
  }

  .reservation-button {
    width: 100%;
  }

  #actions {
    text-align: center;
  }

  .text {
    color: #5c5c5c;
  }

  h4.text {
    margin-bottom: 0px;
  }

  h5.text {
    /* margin-bottom: 0px; */
  }

  .brown {
    color: #8b572a;
    margin-top: 0px;
  }

  .blue {
    background-color: #0d8fb3;
    color: white;
  }

  .highlighted {
    color: #eb9507;
  }

  .dollarsign {
    display: inline;
    font-weight: lighter;
  }
</style>