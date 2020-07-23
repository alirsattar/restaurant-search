<template>
  <div id="searchinputbox" class="mt-5 mb-5">
    <form id="formcontainer">
      <div id="inputfieldscontainer" class="row d-flex">

        <!-- SEARCH FIELDS OUTER CONTAINER -->
        <div class="col-sm-12 col-md-10 col-lg-11">
          <div class="row">

            <!-- NAME INPUT -->
            <div class="col-sm-12 col-md-3 col-lg-3 col-xl-3 d-flex inputcontainer">
              <div class="icon-container">
                <i class="fas fa-home blue"></i>
              </div>
              <div class="input-container">
                <input
                  type="text"
                  id="name"
                  placeholder="Restaurant Name"
                  v-model="searchTypes.name.queryString" />
              </div>
            </div>

            <!-- CITY INPUT -->
            <div class="col-sm-12 col-md-3 col-lg-3 col-xl-3 d-flex inputcontainer">
              <div class="icon-container">
                <i class="fas fa-city blue"></i>
              </div>
              <div class="input-container">
                <input
                  type="text"
                  id="city"
                  placeholder="City"
                  v-model="searchTypes.city.queryString" />
              </div>
            </div>

            <!-- STATE DROPDOWN -->
            <div class="col-sm-12 col-md-3 col-lg-3 col-xl-3 d-flex inputcontainer">
              <div class="dropdown">
                <button
                  class="btn btn-light dropdown-toggle px-0"
                  type="button"
                  id="state-dropdown"
                  data-toggle="dropdown"
                  aria-haspopup="true"
                  aria-expanded="false">
                    <div class="icon-container">
                      <i class="fas fa-map blue"></i>
                    </div>
                    <div id="state-label-container">
                      <span id="dropdown-label">State</span>
                    </div>
                </button>
                <div
                  class="dropdown-menu scrollable-menu"
                  aria-labelledby="dropdownMenuButton">
                  <a
                    v-for="(stateName, stateCode) in usStates"
                    class="dropdown-item"
                    v-bind:key="stateCode"
                    :value="stateCode"
                    @click="selectState(stateCode)">
                      {{ stateName }}
                  </a>
                </div>
              </div>
            </div>

            <!-- ZIP INPUT -->
            <div class="col-sm-12 col-md-3 col-lg-3 col-xl-3 d-flex inputcontainer">
              <div class="icon-container">
                <i class="fas fa-map-marker-alt blue"></i>
              </div>
              <div class="input-container">
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
        </div>

        <!-- SEARCH BUTTON -->
        <div class="col-sm-12 col-md-2 col-lg-1 searchbutton-container">
          <button
            id="searchbutton"
            type="submit"
            class="btn btn-info blue"
            v-on:click="onSearch($event)">
              <i class="fas fa-search"></i> Search
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
  export default {
    name: "SearchInputBox",
    data: () => {
      return {
        searchApiBaseUrl: "https://opentable.herokuapp.com/api/restaurants",
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

        const whichField = this.whichInputFieldToUse();

        if (!whichField) {
          return;
        }

        const urlWithQuery = this.buildFinalQueryUrl(whichField);

        this.$emit('gotSearchQuery', urlWithQuery);
      },
      clearOtherInputFields: function(focusEvent) {
        focusEvent.preventDefault();
      },
      /**
       * @returns {('name' | 'city' | 'state' | 'zip')}
       */
      whichInputFieldToUse: function() {
        let whichField;
        
        const allInputFields = document.getElementById('searchinputbox').querySelectorAll('input');
        let fieldsWithInput = [];

        allInputFields.forEach((field)=> {
          if(field.value) {
            fieldsWithInput.push(field.id);
          }
        });

        if (fieldsWithInput.length) {

          // Just take the first field with input, if multiple fields have been filled in
          whichField = fieldsWithInput[0];
        } else {
          if (this.$data.searchTypes.state.queryString.length) {
            whichField = 'state';
          }
        }

        return whichField;
      },
      buildFinalQueryUrl: function(whichField) {
        const baseUrl = this.$data.searchApiBaseUrl;
        const queryParam = this.$data.searchTypes[whichField].queryParam;
        const searchString = this.$data.searchTypes[whichField].queryString;

        const finalUrl = baseUrl + queryParam + searchString;

        return finalUrl;
      },
      selectState: function(stateCode) {
        this.$data.searchTypes.state.queryString = stateCode;
      }
    }
  };
</script>

<style scoped>
  @media screen and (max-width: 767px) {
    #searchbutton {
      border-radius: 2px;
      border-top-left-radius: 0px;
      border-top-right-radius: 0px;
    }
  }

  @media screen and (min-width: 768px) {
    #searchbutton {
      border-top-left-radius: 0px;
      border-bottom-left-radius: 0px;
    }
  }

  #inputfieldscontainer {
    border-radius: 2px;
    background-color: white;
    align-items: center;
    box-shadow: 0px 1px 5px #888888;
    min-height: fit-content;
  }

  .inputcontainer {
    align-items: center;
    align-self: center;
  }

  #error {
    color: red;
  }

  input {
    border: none;
    /* border-bottom: 1px solid #e6e6e6; */
    width: -webkit-fill-available;
    font-size: large;
    font-weight: 600;
    background-color: 0 0 0 0;
  }

  input:focus, button:focus, .dropdown:focus {
    outline: none;
  }

  .searchbutton-container {
    height: 3em;
    padding: 0;
  }

  #searchbutton {
    width: 100%;
    height: inherit;
    font-size: large;
    font-weight: 600;
  }

  .blue {
    color: #1090b4;
    font-size: x-large;
  }

  button.blue {
    background-color: #1090b4;
    color: white;
  }

  .no-pad-right {
    padding-right: 0px;
  }

  #state-dropdown {
    background-color: white;
    border: none;
  }

  .scrollable-menu {
    width: -webkit-fill-available;
    height: auto;
    max-height: 200px;
    overflow-x: hidden;
    border-radius: 5px;
  }

  #state-label-container {
    width: -webkit-fill-available;
    border: none;
    border-bottom: 1px solid #e6e6e6;
    padding-left: 2px;
  }

  #dropdown-label {
    float: left;
    border: none;
    color: #75797e;
    font-size: large;
    font-weight: 600;
  }

  .dropdown, .input-container {
    width: -webkit-fill-available;
  }

  .dropdown-toggle {
    width: -webkit-fill-available;
    display: flex;
    align-items: center;
  }

  .icon-container {
    width: 15%;
    min-width: 40px;
    display: flex;
    justify-content: center;
    justify-self: left;
  }

  i {
    justify-self: left;
  }

  .input-container {
    justify-self: left;
    border: none;
    border-bottom: 1px solid #e6e6e6;
  }
</style>