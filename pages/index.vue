<template>
  <div class="main-app">
    <div class="container">
      <span class="title d-flex">
        <h1 class="text-white">Weather App</h1>
      </span>
      <div class="header">
        <b-form-input id="input-large" size="lg" placeholder="Tehran" class="rounded-pill" />
      </div>
      <div class="weather-main d-flex flex-row">
        <div class="city-info d-flex flex-column p-4">
          <h2 class="city-name">
            Tehran
          </h2>
          <span class="time-zone">September 25,2020</span>
          <img class="main-weather-img" src="../assets/svg/003-cloudy-4.svg" alt="">
          <span>Cloudy</span>
        </div>
        <div class="weather-details">
          <b-tabs content-class="mt-3">
            <b-tab title="Hourly" active>
              <p>I'm the first tab</p>
            </b-tab>
            <b-tab title="Daily">
              <p>I'm the second tab</p>
            </b-tab>
            <b-tab title="Details">
              <p>I'm a disabled tab!</p>
            </b-tab>
            <b-tab title="Precpitation">
              <p>I'm a Precpitation tab!</p>
            </b-tab>
          </b-tabs>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style>
*,body,html{
  font-family: Montserrat,sans-serif !important;
  box-sizing: border-box;
  margin: 0px;
  padding: 0px;
}
.main-app{
  overflow: hidden;
  height: 100%;
  background: url('../assets/pic/weather.PNG') no-repeat;
  background-size: 100% 100%;
  object-fit: fill;

}
#input-large{
  padding-left: 70px;
  background-color: white !important;
  background: url('../assets/svg/search.svg') 20px 10px no-repeat;
  background-size: 25px 25px;
  width: 60%;
  margin: auto;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;

}
.header{
  width: 100%;
  margin: 50px auto 50px auto;
}
.text-white{
  margin:48px auto 0px auto;
}.weather-main{
  color: white;
  margin:10px auto;
  width: 100%;
  height: 382px;
  background-color: rgba(255, 255, 255, 0.096);
  border-radius: 10px;
}
.main-weather-img{
  height: 100px;
  width:100px;
}
.weather-details{
  padding: 10px;
}
a{
  color: white ;
}
a:hover{
color:white ;
}
.nav-tabs .nav-link:hover, .nav-tabs .nav-link:focus {
  border: none;
    border-bottom: 3px solid #fff !important;
}
.nav-tabs .nav-link.active, .nav-tabs .nav-item.show .nav-link {
    color: #fff;
    border:none;
}
</style>
