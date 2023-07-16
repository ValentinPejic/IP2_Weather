<template>
    <div class="days-tab text-center">
      <div v-if="loading" class="loading">Loading...</div>
      <ul v-else class="p-0">
        <li v-for="(day, index) in forecast" :key="index" class="li_active">
          <div class="py-3"><img :src="day.iconURL"></div>
          <div class="py-3">{{ getDayName(index) }}</div>
          <div class="py-3">{{ day.temperature }}&deg;C</div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import moment from 'moment';
  
  export default {
    props: {
      cityname: String
    },
    data() {
      return {
        forecast: [],
        loading: true,
        iconURL: null
      };
    },
    mounted() {
      this.fetchWeatherData();
    },
    methods: {
      async fetchWeatherData() {
        const apiKey = 'f2cf5e8380035c1adf33499e18460b53';
        const city = this.cityname;
        const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;
  
        try {
          const response = await axios.get(apiUrl);
          const forecastData = response.data.list;
  
          const filteredData = forecastData
            .map(item => {
              return {
                date: moment(item.dt_txt.split(' ')[0]).format('YYYY-MM-DD'),
                temperature: Math.round(item.main.temp),
                description: item.weather[0].description,
                iconURL: `https://openweathermap.org/img/wn/${item.weather[0].icon}.png`
              };
            })
            .reduce((acc, item) => {
              if (!acc.some(day => day.date === item.date)) {
                acc.push(item);
              }
              return acc;
            }, [])
            .slice(1, 5);
  
          console.log(filteredData, "working");
          this.forecast = filteredData;
          this.loading = false;
        } catch (error) {
          console.error('Error fetching data:', error);
          this.loading = false;
        }
      },
      getDayName(index) {
        const daysOfWeek = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
        const today = new Date().getDay();
        const nextDayIndex = (today + index + 1) % 7;
        return daysOfWeek[nextDayIndex];
      }
    }
  };
  </script>
  



<style>

    .days-tab{
        width: 90%;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
        border-radius: 20px;
        width: 90%;
        margin: auto;
    }

    .loading{
        color: #fff;
    }

    ul{
        margin: 0;
    }

    li{
        display: inline-block;
        list-style: none;
        height: 100%;
        width: 21%;
        max-width: 21%;
        font-size: 1vw;
        line-height: 1.2;
    }

    span{
        display: block;
        margin-bottom: 5px;
        height: 35px;
    }

    .li_active{
        background: #253d5c;
        color: #222931;
        border-radius: 10px;
        margin: 0.5rem;
        color: #fff;
        font-weight: 600;
    }

    .li_active:hover{
        transform: scale(1.2);
        transition: transform 0.1s ease;
    }

    .li_active_temp{
        display: inline-block;
        background-color: #222831;
        color: #fff;
        transition: background-color0.5s;
        border-radius: 10px;
    }

    .li_active_temp:hover{
        transform: scale(1.2);
        transition: transform 0.1s ease;
        background: #fff;
        border-radius: 10px;
        color: #191a1f;
    }

</style>
  