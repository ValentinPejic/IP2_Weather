<template>
  <div>
    <h1 id="title">Shows temperature every 3h</h1>
    <div class="searchbar">
      <input type="text" id="txt" v-model="city" placeholder="Enter a city">
      <button id="getTemperatureButton" @click="getTemperature">Get Temperature</button>
    </div>
    <div v-if="temperatures.length > 0">
      <h2 id="utekst">Temperature:</h2>
      <div v-for="temperature in paginatedTemperatures" :key="temperature.dt">
        <p id="tmp">{{ temperature.dt_txt }} - {{ temperature.main.temp }}°C</p>
      </div>
      <div>
        <button @click="prevPage" :disabled="page === 1">Previous</button>
        <button @click="nextPage" :disabled="page === totalPages">Next</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      temperatures: [],
      page: 1,
      perPage: 5,
      totalPages: 0
    };
  },
  computed: {
    paginatedTemperatures() {
      const startIndex = (this.page - 1) * this.perPage;
      const endIndex = startIndex + this.perPage;
      return this.temperatures.slice(startIndex, endIndex);
    }
  },
  methods: {
    async getTemperature() {
      try {
        const response = await fetch(`https://api.openweathermap.org/data/2.5/forecast?q=${this.city}&units=metric&appid=f2cf5e8380035c1adf33499e18460b53`);
        const data = await response.json();
        const currentDate = new Date();
        const currentHour = currentDate.getHours();
        const startIndex = Math.floor((currentHour) / 3) - 1;
        this.temperatures = data.list.slice(startIndex, startIndex - 25);
        this.totalPages = Math.ceil(this.temperatures.length / this.perPage);
        this.page = 1;
      } catch (error) {
        console.log(error);
      }
    },
    nextPage() {
      if (this.page < this.totalPages) {
        this.page++;
      }
    },
    prevPage() {
      if (this.page > 1) {
        this.page--;
      }
    }
  },
  created() {
    const weatherApp = document.querySelector('.app');
    if (weatherApp) {
      weatherApp.style.display = 'none';
    }
  },
  unmounted() {
    const weatherApp = document.querySelector('.app');
    if (weatherApp) {
      weatherApp.style.display = 'block';
    }
  }
};
</script>


<style>

#title{
  color: white;
  text-align: center;
  font-size: 50px;
  border-radius: 20px;
}

#txt {
  align-items: center;
  justify-content: center;
  align-items: center;
  width: 40vh;
  height: 4.5vh;
  text-align: center;
}

#utekst{
  color: white;
}

#tmp{
  color: white;
  font-weight: 200;
}


#getTemperatureButton {
  padding: 10px 20px;
  border-radius: 4px;
  border: none;
  background-color: #3f51b5;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

#getTemperatureButton:hover {
  background-color: #5c6bc0;
}

#getTemperatureButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}


#baza{
  background-color: rgb(161, 102, 102);
}

</style>





