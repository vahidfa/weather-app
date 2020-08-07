<template>
  <div class="main-app">
    <div class="container">
      <span class="title d-flex">
        <h1 class="text-white">Weather App</h1>
      </span>
      <div class="header">
        <b-form-input
          id="input-large"
          v-model="city"
          size="lg"
          placeholder="Tehran"
          class="rounded-pill"
          @keyup.enter="newWeatherHandle"
        />
      </div>
      <div class="weather-container d-flex flex-column">
        <div class="weather-main d-flex flex-row justify-content-between">
          <div class="city-info d-flex flex-column pl-5 pt-3">
            <h2 v-for="(item, dt) in weather" :key="dt" class="city-name">
              {{ item.name }}
            </h2>
            <span class="time-zone">{{ date }}</span>
            <img v-for="(item, main) in weather" :key="main" class="main-weather-img" :src="img" alt="">
            <span v-for="(item, id) in weather" :key="id">{{ item.weather[0].description }}</span>
          </div>
          <div class="weather-temp">
            <p v-for="(item, index) in weather" :key="index">
              {{ item.main.temp | FtoC }}°
            </p>
            <span v-for="(item, name) in weather" :key="name">{{ item.main.temp_max | FtoC }}°</span>/ <span v-for="(item, index) in weather" :key="index">{{ item.main.temp_min | FtoC }}°</span>
          </div>
        </div>
        <div class="weather-details">
          <b-tabs content-class="mt-3">
            <b-tab title="Hourly" active class="d-flex flex-row">
              <div v-for="(item, index) in hourlyList" :key="index" class="d-flex flex-column m-auto">
                <p>
                  {{ item.dt_txt | showHours }}
                </p>
                <img src="../assets/svg/003-cloudy-4.svg" alt="" class="weather-details-img">
                <p>{{ item.main.temp | FtoC }}°</p>
              </div>
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
import axios from 'axios'
export default {
  filters: {
    FtoC (value) {
      return Math.floor(value - 273.15)
    },
    showHours (value) {
      const now = new Date(value)
      return now.getHours() + ':' + now.getMinutes() + '0'
    }
  },
  data () {
    return {
      city: '',
      weather: [],
      img: '',
      date: '',
      hourlyList: ''
    }
  },
  mounted () {
    this.getCurrentWithHourlyForStartUp()
  },
  methods: {
    sendCity () {
      axios
        .get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=75b0312ef38365bd3c02771213293312`)
        .then((response) => {
          this.weather.push(response.data)
          this.img = 'http://openweathermap.org/img/wn/' + response.data.weather[0].icon + '@2x.png'
          console.log(this.weather)
          console.log(this.img)
        })
        .catch((error) => {
          console.log(error)
        })
    },
    getWeather () {
      try {
        return axios
          .get('https://api.openweathermap.org/data/2.5/weather?q=Tehran&appid=75b0312ef38365bd3c02771213293312')
      } catch (error) {
      }
    },
    getWeatherBasedOnCity (info) {
      return axios
        .get(`https://api.openweathermap.org/data/2.5/forecast?q=${info.name}&appid=75b0312ef38365bd3c02771213293312`)
    },
    async getCurrentWithHourlyForStartUp () {
      const response = await this.getWeather()
      this.weather.push(response.data)
      this.img = 'http://openweathermap.org/img/wn/' + response.data.weather[0].icon + '@2x.png'
      const dateObj = new Date()
      const month = dateObj.getUTCMonth() + 1
      const day = dateObj.getUTCDate()
      const year = dateObj.getUTCFullYear()
      this.date = year + ' / ' + month + ' / ' + day
      const data = await this.getWeatherBasedOnCity(response.data)
      const list = data.data.list.splice(1, 5)
      this.hourlyList = list
      console.log(this.hourlyList)
      console.log(data.data.list)
    },
    newWeatherHandle (info) {
      this.weather = []
      this.hourlyList = ''
      this.sendCity()
      this.getWeatherBasedOnCity(info)
      this.getCurrentWithHourlyForStartUp()
    }
  }
}
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
}.weather-container{
  color: white;
  margin:10px auto;
  width: 100%;
  /* height: 382px; */
  background-color: rgba(255, 255, 255, 0.096);
  border-radius: 10px;
}
.main-weather-img{
  height: 150px;
  width: 150px;
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
    color: #000;
    border:none;
}
.weather-temp span{
  font-size: 48px;
}
.weather-temp p{
  font-size: 100px;
  margin: 0;
}
.weather-temp {
  padding: 20px 48px;
}
.weather-details-img{
  height: 50px;
  width: 50px;
}
p{
  margin: 0;
}
</style>
