<template>
  <div id="appheader">
    <div id="header-container" class="container-fluid">
      <div class="row">

        <!-- LEFT SIDE -->
        <div class="col-6 col-sm-12">
          <img
            id="banner"
            src="images/hungrytime_banner.jpg"
            class="float-left">
        </div>

        <!-- RIGHT SIDE -->
        <div class="col-6 col-sm-12">
          <ul id="nav-ul" class="list-group list-group-horizontal float-right">
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
      </div>
    </div>

    <!-- MESSAGE BANNER -->
    <div class="row">
      <div class="col-12">
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
  props: {},
  components: {}
};
</script>

<style scoped>
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
    padding: 0vw 5vw;
  }

  #banner {
    max-height: 5rem;
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