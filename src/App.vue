<template>
  <div id="main" :class="isDay? 'day':'night'">
    <div class="container my-5">
        <h1 class="text-container">Weather in</h1>
        <form class="search-location">
            <input
            type="text" 
            name="city" 
            placeholder="What city?"
            autocomplete="off"
            v-model="citySearch"
            class="form-control text-muted form-rounded p-4 shadow-sm">
            <button class="search-btn p-4" v-on:click.prevent="getWeather">Search</button>
        </form>
        <div class="card my-3 shadow-lg back-card" :class="{'d-none':visability}">
          <div :class="isDay? 'card-day' : 'card-night'">
          <div class="card-top text-center">
              <div class="city-name my-3">
                  <p>{{weather.cityName}}</p>
                  <p class="country">{{weather.country}}</p>
              </div>
          </div>
          <div class="card-body">
              <div class="card-mid row">
                <ul class="card-list text-center">

                  <li class="card-column">
                    <weather-col v-for="res in this.weather" 
                    :key="res.id"
                    :day="weather.day"
                    :temperature="weather.temperature"
                    :description="weather.description"
                    :high="weather.highTemp"
                    :low="weather.lowTemp"
                    :clouds="weather.clouds"
                    :wind="weather.speedOfWind"
                    class="column">
                    </weather-col>
                  </li>
                </ul>  
              </div>
          </div>
          </div>
        </div>
    </div>
  </div>
</template>

<script>

import WeatherCol from './components/weather-col.vue'

export default {
  components: {
    WeatherCol
  },

  data() {
    return {
      visability: true,
      errorMessage: "",
      isDay: true,
      citySearch: "",
      weather: {
        cityName: "",
        country: "",
        day: "",
        temperature: 0,
        description: "",
        lowTemp: "",
        highTemp: "",
        clouds: "",
        speedOfWind: ""
      },
    };
  },
  methods: {
    getWeather: async function() {
      if(this.citySearch != "") {
        console.log(this.citySearch)
        this.visability = false
      }
      
      const key = '63623439096e87e393efaaa735fcb9cd'
      const baseURL = `https://api.openweathermap.org/data/2.5/forecast?q=${this.citySearch}&appid=${key}&units=metric`
      const response = await fetch(baseURL)
      const data = await response.json()
      console.log(data)
      

      for (let i=0; i<5; i++) {
        this.citySearch = ""
        this.weather.cityName = data.city.name
        this.weather.country = data.city.country
        this.weather.day = String(data.list[i].dt_txt.substring(8,10))+"."+String(data.list[i].dt_txt.substring(5,7))
        this.weather.temperature = Math.round(data.list[i].main.temp)
        this.weather.description = data.list[i].weather[0].description
        this.weather.highTemp = Math.round(data.list[i].main.temp_max)
        this.weather.lowTemp = Math.round(data.list[i].main.temp_min)
        this.weather.clouds = data.list[i].clouds.all
        this.weather.speedOfWind = Math.round(data.list[i].wind.speed)

        const timeOfDay = data.list[i].weather[0].icon

        if(timeOfDay.includes("n")) {
          this.isDay = false
        } else {
          this.isDay = true
        }
        console.log(this.weather)
      }
    },
  },
};
</script>

<style>
  @import "./assets/style.css";
</style>
