<template>
  <div id="header-container" class="container-fluid">
    
    <!-- NAV ROW -->
    <div id="nav-row" class="row">

      <!-- LEFT SIDE -->
      <div class="col-sm-12 col-md-6 col-lg-6">
        <img
          id="banner"
          src="images/hungrytime_banner.jpg"
          class="float-left">
      </div>

      <div class="col-sm-12 col-md-6 col-lg-6">

        <nav class="navbar navbar-expand-md navbar-light bg-white responsive-float">

          <div class="" id="navbarSupportedContent">
            <ul class="navbar-nav">
              <li class="my-auto mr-4">
                <h3><a><strong>My Dashboard</strong></a></h3>
              </li>
              <li class="my-auto mr-4">
                <h3><a><strong>Search</strong></a></h3>
              </li>
              <li class="my-auto mr-3"><i class="fas fa-bell"></i></li>
              <li class="my-auto"><img src="images/hungrytime_usericon.png" alt=""></li>
            </ul>
          </div>
        </nav>

      </div>

    </div>

    <!-- MESSAGE BANNER -->
    <div id="disappearing-margin" class="row">
      <div id="banner-row" class="col-12">
        <div id="banner-container">
          <h5 id="message-banner" class=""><strong>Enjoy our discounts! Create an account <a href="">here</a></strong></h5>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "AppHeader",
  };
</script>

<style scoped>
  @media screen and (max-width: 767px){
    #disappearing-margin {
      margin-bottom: 0em;
    }
  }

  @media screen and (min-width: 768px){
    #disappearing-margin {
      margin-bottom: 3em;
    }

    .responsive-float {
      float: right;
    }
    
    h3 {
      font-size: 1.25rem;
    }
  }

  @media screen and (min-width: 1000px){
    h3 {
      font-size: 2rem;
    }
  }

  #appheader {
    padding: 0px;
    margin: 0px;
    margin: 0px;
    background-color: #ffffff;
    height: fit-content;
    width: 100%;
  }

  #nav-ul {
    list-style-type: none;
  }

  li {
    float: left;
  }

  #header-container {
    background-color: #ffffff;
  }

  #nav-row {
    padding: 0px 5vw;
  }

  #banner {
    max-height: 5rem;
  }

  #banner-row {
    padding: 0px;
  }

  #banner-container {
    display: flex;
    text-align: center;
    background-color: #cceff7;
  }

  #message-banner {
    text-align: center;
    width: 100vw;
    padding: 10px 0px;
    margin-bottom: 0px;
  }
</style>