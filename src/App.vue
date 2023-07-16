<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">Detailed</router-link>
  </nav>
  <router-view/>

  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">Weather app</h1>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input type="text" class="input form-control" v-model="city" placeholder="Enter a city">
        </div>
        <button class="btn-search btn btn-primary" @click="searchWeather">Search <i class="fas fa-search"></i></button>
      </div>
    </div>
    <Weather :city="city" v-if="showWeather"></Weather>
  </div>
</template>


<script>

 import Weather from './components/Weather.vue'

 export default (await import('vue')).defineComponent({
  name: 'App',
  components: {
    Weather
  },
  data() {
    return {
      city: "",
      showWeather: false,
    };
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      this.$nextTick(async () => {
        this.showWeather = true;
      });
    },
  },
 });

</script>




<style>

body{
  background-color: #121212 !important;
}

.header{
  background-color: #212730;
  border-radius: 20px;
  color: #fff;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  margin-top: 5rem;
}

.btn-search {
  background-image: linear-gradient(to right, #780206, #061161);
}


</style>
