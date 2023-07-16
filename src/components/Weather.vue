<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">Today</h2>
          <p class="date mb-0">{{ date }}</p>
          <small>{{ time }}</small>
          <h2 class="place"><i class="fa fa-location">{{ name }} <small>{{ country }}</small></i></h2>
          <div class="temp" style="position: absolute; bottom: 10px; left: 20px;">
            <h1 class="weather-temp">{{ temperature }}&deg;</h1>
            <h2 class="text-align">{{ description }} <img :src="iconURL"></h2>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
        <table class="m-4">
          <tbody>
            <tr>
              <th>Sea level</th>
              <td v-if="sea_level>0">{{sea_level}}</td>
              <td v-else>Null</td>
            </tr>
            <tr>
              <th>Humidity</th>
              <td>{{Humidity}}</td>
            </tr>
            <tr>
              <th>Wind</th>
              <td>{{wind}}</td>
            </tr>
          </tbody>
        </table>

        <DaysWeather :cityname="cityname"></DaysWeather>
  
        <div id="div_Form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input type="button" value="Change Location" @click="changeLocation" class="btn btn-search btn-primary">
          </form>
        </div>
      </div>  
    </div>
      
    </div>
  </template>
  
  
  <script>
  import axios from 'axios';
  import DaysWeather from './DaysWeather.vue';
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'myWeather',
    components: {
      DaysWeather,
    },
    props: {
      city: String,
    },
    data() {
      return {
        cityname: this.city,
        temperature: null,
        description: null,
        iconCode: null,
        iconURL: null,
        date: null,
        time: null,
        name: null,
        sea_level: null,
        humidity: null, // Updated variable name
        wind: null,
        country: null,
        monthNames: ['January', 'February', 'March', 'April', 'June', 'July', 'August', 'September', 'November', 'December'],
      };
    },
    methods: {
      changeLocation() {
        window.location.reload();
      },
    },
    async created() {
      const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=f2cf5e8380035c1adf33499e18460b53`);
      const weatherData = response.data;
      this.temperature = Math.round(weatherData.main.temp);
      this.description = weatherData.weather[0].description;
      this.iconCode = weatherData.weather[0].icon;
      this.iconURL = `https://openweathermap.org/img/wn/${this.iconCode}.png`;
      this.name = weatherData.name;
      this.wind = weatherData.wind.speed;
      this.sea_level = weatherData.main.sea_level; // Updated variable name
      this.country = weatherData.sys.country;
      this.Humidity = weatherData.main.humidity.toString(); // Updated variable name
      const d = new Date();
      this.date = d.toLocaleString('en-US', { month: 'long', day: 'numeric', year: 'numeric' });
      this.time = d.toLocaleString('en-US', { hour: 'numeric', minute: 'numeric', second: 'numeric' });
      console.log(weatherData);
    },
  });
  </script>
  
  
  


<style>

  body{
    background-color: #343d4b;
  }

  .weather-temp{
    margin: 0;
    font-weight: 500;
    font-size: 4em;
    text-align: left;
  }

  h2.mb-1.day{
    font-size: 3rem;
    font-weight: 600;
  }

  .main-div {
  border-radius: 20px;
  color: #fff;
  background-image: url("https://images.unsplash.com/photo-1437719417032-8595fd9e9dc6?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1074&q=80");
  background-size: cover;
  background-position: center;
}

 .main-div:hover{
    transform: scale(1.1);
    transition: transform 0.5s ease;
    z-index: 1;
 }

 .card-2{
    background-color: #212730 !important;
 }

 table{
    position: relative;
    left: 15px;
    border-collapse: separate;
    border-spacing: 15px;
    width: 85%;
    text-align: left;
    max-width: 600px;
    margin: 0 auto;
 }

 th,td{
    font-size: 15px;
    font-weight: 400;
    color: #fff;
    background-color: #212730;
 }

 .card-details{
    margin-left: 19px;
 }

 .h1_left{
    position: absolute;
    bottom: 25px;
    left: 16px;
    font-size: 3vw;
    line-height: 1.2;
 }


</style>
  