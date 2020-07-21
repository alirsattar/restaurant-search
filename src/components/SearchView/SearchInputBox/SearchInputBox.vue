<template>
  <form id="formcontainer">
    <div id="searchinputbox">
      <div id="inputfieldscontainer" class="row d-flex">
          <div class="col-10">
            <div class="row">

              <!-- NAME INPUT -->
              <div class="col-3 d-flex inputcontainer">
                <i class="fas fa-home mr-2 blue"></i>
                <input
                  type="text"
                  id="name"
                  placeholder="Restaurant Name"
                  v-model="searchTypes.name.queryString" />
              </div>

              <!-- CITY INPUT -->
              <div class="col-3 d-flex inputcontainer">
                <i class="fas fa-city mr-2 blue"></i>
                <input
                  type="text"
                  id="city"
                  placeholder="City"
                  v-model="searchTypes.city.queryString" />
              </div>

              <!-- STATE INPUT -->
              <div class="col-3 d-flex inputcontainer">
                <i class="fas fa-map mr-2 blue"></i>
                <select
                  class="select"
                  id="state"
                  placeholder="State"
                  v-model="searchTypes.state.queryString">
                    <option
                      v-for="(code, state) in usStates"
                      v-bind:key="state"
                      :value="state">{{ code }}</option>
                </select>
              </div>

              <!-- ZIP INPUT -->
              <div class="col-3 d-flex inputcontainer">
                <i class="fas fa-map-marker-alt mr-2 blue"></i>
                <input
                  type="text"
                  id="zip"
                  class=""
                  maxlength="5"
                  placeholder="Zip Code"
                  pattern="[0-9]*"
                  v-model="searchTypes.zip.queryString" />
              </div>
            </div>
          </div>

          <!-- SEARCH BUTTON -->
          <div class="col-2">
            <button
              id="searchbutton"
              type="submit"
              class="btn btn-info ml-3"
              v-on:click="onSearch($event)">
                <i class="fas fa-search"></i> Search
            </button>
          </div>
          <br><small id="error">{{ this.$data.errorMessage }}</small>
      </div>
    </div>
  </form>
</template>

<script>
// const axios = require("axios");

export default {
  name: "SearchInputBox",
  props: {},
  data: () => {
    return {
      searchApiBaseUrl: "http://opentable.herokuapp.com/api/restaurants",
      searchTypes: {
        name: {
          queryParam: "?name=",
          queryString: ""
        },
        city: {
          queryParam: "?city=",
          queryString: ""
        },
        state: {
          queryParam: "?state=",
          queryString: ""
        },
        zip: {
          queryParam: "?zip=",
          queryString: ""
        }
      },
      restaurantNameQuery: "",
      restaurantZipQuery: "",
      restaurantCityQuery: "",
      restaurantStateQuery: "",
      finalQueryParam: "",
      errorMessage: '',
      resultCount: 0,
      usStates: {
        "": "State",
        "AL": "Alabama",
        "AK": "Alaska",
        "AZ": "Arizona",
        "AR": "Arkansas",
        "CA": "California",
        "CO": "Colorado",
        "CT": "Connecticut",
        "DE": "Delaware",
        "DC": "District of Columbia",
        "FL": "Florida",
        "GA": "Georgia",
        "HI": "Hawaii",
        "ID": "Idaho",
        "IL": "Illinois",
        "IN": "Indiana",
        "IA": "Iowa",
        "KS": "Kansas",
        "KY": "Kentucky",
        "LA": "Louisiana",
        "ME": "Maine",
        "MD": "Maryland",
        "MA": "Massachusetts",
        "MI": "Michigan",
        "MN": "Minnesota",
        "MS": "Mississippi",
        "MO": "Missouri",
        "MT": "Montana",
        "NE": "Nebraska",
        "NV": "Nevada",
        "NH": "New Hampshire",
        "NJ": "New Jersey",
        "NM": "New Mexico",
        "NY": "New York",
        "NC": "North Carolina",
        "ND": "North Dakota",
        "OH": "Ohio",
        "OK": "Oklahoma",
        "OR": "Oregon",
        "PA": "Pennsylvania",
        "RI": "Rhode Island",
        "SC": "South Carolina",
        "SD": "South Dakota",
        "TN": "Tennessee",
        "TX": "Texas",
        "UT": "Utah",
        "VT": "Vermont",
        "VA": "Virginia",
        "WA": "Washington",
        "WV": "West Virginia",
        "WI": "Wisconsin",
        "WY": "Wyoming"
      }
    };
  },
  methods: {
    onSearch: function(buttonEvent) {
      buttonEvent.preventDefault();

      // TODO: Function to figure out which input field's string will be appended to URL
      const whichField = this.whichInputFieldToUse();
      const urlWithQuery = this.buildFinalQueryUrl(whichField);

      console.log({ urlWithQuery });

      this.$emit('gotSearchQuery', urlWithQuery);
    },
    clearOtherInputFields: function(focusEvent) {
      focusEvent.preventDefault();
      // const inputFields = document.getElementById('searchinputbox').querySelectorAll('input');
      // const fieldBeingFocusedId = focusEvent.srcElement.id;

      // inputFields.forEach((field) => {
      //   if(field.id !== fieldBeingFocusedId) {
      //     console.log('starting field value: ', field.value);

      //     field.value = '';

      //     console.log('ending field value: ', field.value);
      //   }
      // })

      // // TODO: Clear the other input field
      // console.log({inputFields, fieldBeingFocusedId});
    },
    whichInputFieldToUse: function() {
      let whichField;
      
      const allInputFields = document.getElementById('searchinputbox').querySelectorAll('input, select');
      let fieldsWithInput = [];

      allInputFields.forEach((field)=> {
        if(field.value) {
          fieldsWithInput.push(field.id);
        }
      });

      if (fieldsWithInput.length) {
        // Just take the first field with input, if multiple fields have been filled in
        whichField = fieldsWithInput[0];
      }

      return whichField;
    },
    buildFinalQueryUrl: function(whichField) {
      const baseUrl = this.$data.searchApiBaseUrl;
      const queryParam = this.$data.searchTypes[whichField].queryParam;
      const searchString = this.$data.searchTypes[whichField].queryString;

      const finalUrl = baseUrl + queryParam + searchString;

      console.log({ baseUrl, queryParam, searchString });

      return finalUrl;
    }
  }
};
</script>

<style scoped>
  #searchinputbox {
    padding: 10px;
    margin: 50px 10px 10px 10px;
  }

  #inputfieldscontainer {
    border-radius: 2px;
    background-color: white;
    align-items: center;
    box-shadow: 0px 1px 3px #888888;
  }

  .inputcontainer {
    align-items: center;
    align-self: center;
  }

  #error {
    color: red;
  }

  input, select {
    border: none;
    border-bottom: 1px solid #e6e6e6;
  }

  #searchbutton {
    width: 100%;
    border-radius: 2px;
    border-top-left-radius: 0px;
    border-bottom-left-radius: 0px;
  }

  .blue {
    color: #1090b4
  }
</style>