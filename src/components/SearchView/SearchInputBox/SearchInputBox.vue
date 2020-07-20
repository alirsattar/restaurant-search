<template>
  <div id="searchinputbox">
    <h1>
      <span class="badge badge-success">SearchInputBox</span>
    </h1>

    <form>
      <input
        type="text"
        id="name"
        placeholder="Restaurant Name"
        v-model="searchTypes.name.queryString"
        v-on:focus="clearOtherInputFields($event)" />
      <input
        type="text"
        id="city"
        placeholder="City"
        v-model="searchTypes.city.queryString"
        v-on:focus="clearOtherInputFields($event)" />
      <select
        class="select"
        id="state"
        placeholder="State"
        v-model="searchTypes.state.queryString"
        v-on:focus="clearOtherInputFields($event)">
          <option
            v-for="(code, state) in usStates"
            v-bind:key="state"
            :value="state">{{ code }}</option>
      </select>
      <span class="m-3">OR</span>
      <input
        type="text"
        id="zip"
        class=""
        maxlength="5"
        placeholder="Zip Code"
        pattern="[0-9]*"
        v-model="searchTypes.zip.queryString"
        v-on:focus="clearOtherInputFields($event)" />
      <button
        type="submit"
        class="btn btn-success ml-3"
        v-on:click="onSearch($event)">Search</button>
      <br><small id="error">{{ this.$data.errorMessage }}</small>
    </form>

    <div
      id="resultcount"
      v-show="this.$data.resultCount">
      {{ this.$data.resultCount }} Results
    </div>
    
  </div>
</template>

<script>
const axios = require("axios");

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

      // const whichField = this.whichInputFieldToUse();

      // TODO: Function to figure out which input field's string will be appended to URL
      const whichField = this.whichInputFieldToUse();
      const urlWithQuery = this.buildFinalQueryUrl(whichField);

      // const urlWithQuery = this.$data.searchApiBaseUrl + this.$data.searchString;

      console.log({ urlWithQuery });

      axios.get(urlWithQuery)
        .then((searchResults) => {
          const restaurantResults = searchResults.data.restaurants;
          const totalResults = searchResults.data.total_entries;

          console.log({ restaurantResults, totalResults });

          this.$data.resultCount = totalResults;
          this.$emit('gotSearchResults', restaurantResults);
        })
        .catch((error)=> {
          console.error(error);
          this.$data.errorMessage = error;
        });
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
  border: 1px solid;
  padding: 10px;
  margin: 10px;
}

#error {
  color: red;
}
</style>