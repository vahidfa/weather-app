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
          placeholder="Please write name of country or city and press 'Enter'"
          class="rounded-pill"
          @keyup.enter="getNewWeather"
        />
      </div>
      <div class="weather-container d-flex flex-column">
        <div class="weather-main d-flex flex-row justify-content-between">
          <div class="city-info d-flex flex-column pl-5 pt-3">
            <h2 v-for="(item, dt) in weather" :key="dt" class="city-name">
              {{ item.name }}
            </h2>
            <span class="time-zone">{{ date }}</span>
            <img v-for="(item, main) in weather" :key="main" class="main-weather-img" :src="mainWeatherIcon" alt="">
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
          <!-- <b-tabs content-class="mt-3"> -->
          <div class="hourly d-flex flex-row">
            <span @click="showHourlyTab">Hourly</span>
            <span @click="showDailyTab">Daily</span>
          </div>
          <div v-if="showHoure" key="data" title="Hourly" active class="d-flex flex-row">
            <div v-for="data in hourlyList" :key="data.cod" class="d-flex flex-column m-auto">
              <p>
                {{ data.dt_txt | showHours }}
              </p>
              <img v-for="img in data.weather" :key="img.name" :src="`http://openweathermap.org/img/wn/${img.icon}@2x.png`" alt="" class="weather-details-img">
              <p>{{ data.main.temp | FtoC }}°</p>
            </div>
          </div>
          <div v-if="showDaily" key="item" title="Daily" class="d-flex flex-row">
            <div class="d-flex flex-column m-auto">
              <p>
                {{ dailyList[5].dt_txt | showDate }}
              </p>
              <img :src="`http://openweathermap.org/img/wn/${dailyList[5].weather[0].icon}@2x.png`" alt="" class="weather-details-img">
              <p>{{ dailyList[5].main.temp | FtoC }}°</p>
            </div>
            <div class="d-flex flex-column m-auto">
              <p>
                {{ dailyList[11].dt_txt | showDate }}
              </p>
              <img :src="`http://openweathermap.org/img/wn/${dailyList[11].weather[0].icon}@2x.png`" alt="" class="weather-details-img">
              <p>{{ dailyList[11].main.temp | FtoC }}°</p>
            </div>
            <div class="d-flex flex-column m-auto">
              <p>
                {{ dailyList[17].dt_txt | showDate }}
              </p>
              <img :src="`http://openweathermap.org/img/wn/${dailyList[17].weather[0].icon}@2x.png`" alt="" class="weather-details-img">
              <p>{{ dailyList[17].main.temp | FtoC }}°</p>
            </div>
            <div class="d-flex flex-column m-auto">
              <p>
                {{ dailyList[23].dt_txt | showDate }}
              </p>
              <img :src="`http://openweathermap.org/img/wn/${dailyList[23].weather[0].icon}@2x.png`" alt="" class="weather-details-img">
              <p>{{ dailyList[23].main.temp | FtoC }}°</p>
            </div>
            <div class="d-flex flex-column m-auto">
              <p>
                {{ dailyList[29].dt_txt | showDate }}
              </p>
              <img :src="`http://openweathermap.org/img/wn/${dailyList[29].weather[0].icon}@2x.png`" alt="" class="weather-details-img">
              <p>{{ dailyList[29].main.temp | FtoC }}°</p>
            </div>
          </div>
          <b-tab title="Details">
            <p>I'm a disabled tab!</p>
          </b-tab>
          <b-tab title="Precpitation">
            <p>I'm a Precpitation tab!</p>
          </b-tab>
          <!-- </b-tabs> -->
        </div>
      </div>
    </div>
    <div v-if="loading" class="loading">
      <div class="spinner">
        <div class="rect1" />
        <div class="rect2" />
        <div class="rect3" />
        <div class="rect4" />
        <div class="rect5" />
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
    },
    showDate (value) {
      const now = new Date(value)
      return now.getFullYear() + '/' + now.getMonth() + '/' + now.getDate()
    }
  },
  data () {
    return {
      city: 'tehran',
      weather: [],
      mainWeatherIcon: '',
      hourlyIcon: '',
      date: '',
      hourlyList: '',
      dailyList: '',
      showHoure: true,
      showDaily: false,
      loading: true
    }
  },
  mounted () {
    this.getCurrentWithHourly()
    this.getWeather()
  },
  methods: {
    getWeather () {
      try {
        return axios
          .get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=75b0312ef38365bd3c02771213293312`)
      } catch (error) {
      }
    },
    getWeatherBasedOnCity (info) {
      return axios
        .get(`https://api.openweathermap.org/data/2.5/forecast?q=${info.name}&appid=75b0312ef38365bd3c02771213293312`)
    },
    async getCurrentWithHourly () {
      const response = await this.getWeather()
      this.weather = [response.data]
      this.mainWeatherIcon = 'http://openweathermap.org/img/wn/' + response.data.weather[0].icon + '@2x.png'
      const dateObj = new Date()
      const month = dateObj.getUTCMonth() + 1
      const day = dateObj.getUTCDate()
      const year = dateObj.getUTCFullYear()
      this.date = year + ' / ' + month + ' / ' + day
      const data = await this.getWeatherBasedOnCity(response.data)
      const hourly = data.data.list.splice(1, 5)
      const daily = data.data.list
      this.dailyList = daily
      this.hourlyList = hourly
      this.loading = false
      // console.log(this.hourlyIcon)
      console.log(this.hourlyList)
      console.log(this.dailyList)
    },
    getNewWeather (info) {
      this.getWeatherBasedOnCity(info)
      this.getCurrentWithHourly()
    },
    showDailyTab () {
      this.showDaily = true
      this.showHoure = false
    },
    showHourlyTab () {
      this.showDaily = false
      this.showHoure = true
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
  height: 110vh;
  background: url('../assets/pic/weather.PNG') no-repeat;
  background-size: 100% 100%;
  object-fit: cover;

}
#input-large{
  padding-left: 70px;
  background-color: white !important;
  background: url('../assets/svg/search.svg') 20px 10px no-repeat;
  background-size: 25px 25px;
  width: 70%;
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
  font-size: 30px;
}
.weather-temp p{
  font-size: 80px;
  margin: 0;
}
.weather-details-img{
  height: 50px;
  width: 50px;
}
p{
  margin: 0;
}
.hourly{
  width: 100%;
  border-bottom: 2px solid #fff;
  margin-bottom: 10px ;
}
.hourly span {
  padding-left: 20px;
  cursor: pointer;
}
.spinner {
  margin:20% auto;
  width: 50%;
  height: 10%;
  text-align: center;
  font-size: 10px;
}

.spinner > div {
  background-color: #333;
  height: 100%;
  width: 6px;
  display: inline-block;

  -webkit-animation: sk-stretchdelay 1.2s infinite ease-in-out;
  animation: sk-stretchdelay 1.2s infinite ease-in-out;
}

.spinner .rect2 {
  -webkit-animation-delay: -1.1s;
  animation-delay: -1.1s;
}

.spinner .rect3 {
  -webkit-animation-delay: -1.0s;
  animation-delay: -1.0s;
}

.spinner .rect4 {
  -webkit-animation-delay: -0.9s;
  animation-delay: -0.9s;
}

.spinner .rect5 {
  -webkit-animation-delay: -0.8s;
  animation-delay: -0.8s;
}

@-webkit-keyframes sk-stretchdelay {
  0%, 40%, 100% { -webkit-transform: scaleY(0.4) }
  20% { -webkit-transform: scaleY(1.0) }
}

@keyframes sk-stretchdelay {
  0%, 40%, 100% {
    transform: scaleY(0.4);
    -webkit-transform: scaleY(0.4);
  }  20% {
    transform: scaleY(1.0);
    -webkit-transform: scaleY(1.0);
  }
}
.loading {
  margin: auto;
  top: 0px;
  position: fixed;
  width: 100%;
  height: 1000px;
  background-color: blueviolet;
}
</style>
