<template>
  <div v-if="typeof weather.main != 'undefined'" class="max-w-5xl flex items-center h-auto lg:h-screen flex-wrap mx-auto my-32 lg:my-0">
    
    <!--Main Col-->
    <div id="profile" class="w-full h-1/4 lg:w-3/5 rounded-lg lg:rounded-l-lg lg:rounded-r-none shadow-2xl bg-white opacity-75 mx-6 lg:mx-0">
    

      <div class="p-4 md:p-12 text-center lg:text-left">
				<form @submit.prevent="fetchWeather()" accept-charset="utf-8">
          <input class="border-b-2 focus:outline-none text-xl font-semibold" type="text" v-model="location" placholder="Enter your location"/>
				</form>
      </div>

    </div>
    
    <!--Img Col-->
    <div class="w-full lg:w-2/5 h-full flex items-center">
      <div class="result-area rounded-none lg:rounded-lg shadow-2xl bg-white h-2/5 w-full opacity-75 py-10 px-8">
        <div class="text-5xl font-bold">{{getDate.weekday}}</div>
        <div class="">{{getDate.date}}</div>
        <div class="font-semibold text-xl flex items-center mt-4">
          <i class="text-base fas fa-map-marker-alt" />  <div class="ml-2">{{weather.name}}, {{weather.sys.country}}</div>
        </div>

        <div class="h-full mt-16">
          <div></div>
          <div class="text-9xl font-bold text-center">{{Math.round(weather.main.temp)}}&deg;C</div>
          <div class="text-left mt-8">
            <div class="text-xl font-semibold">{{ weather.weather[0].main }}</div>
            <div>{{ weather.weather[0].description }}</div>
          </div>
        </div>
      </div>
    </div>
    
    
    <!-- Pin to top right corner -->
      <div class="absolute top-0 right-0 h-12 w-18 p-4">
      <button class="js-change-theme focus:outline-none">🌙</button>
      </div>

  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'Home',
	data: () => ({
		api_url: "https://community-open-weather-map.p.rapidapi.com/weather",
		api_key: "87ccd9ad34mshbb5f99356d00e6ap12354cjsn0ce926f0f7c8",
		location: "Cebu",
		weather: {},
    history: [],
	}),
	methods: {
		fetchWeather() {
      axios.defaults.headers = {
        'x-rapidapi-host': 'community-open-weather-map.p.rapidapi.com',
        'x-rapidapi-key': '87ccd9ad34mshbb5f99356d00e6ap12354cjsn0ce926f0f7c8'
      }
      
			axios
				.get(this.api_url, {
          params: {
            units: 'metric',
            q: this.location
          }
        })
				.then((response) => (this.weather = response.data))
				.catch((error) => console.log(error))
        .finally(() => this.history.push(this.location));
		},
	},
	computed: {
    getDate: function () {
      const today = new Date();
      const weekday = moment(today).format('dddd');
      const date = moment(today).format('DD, MMMM YYYY');
      return { weekday, date }
    },
	},
	mounted() {
		this.fetchWeather();
	},
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.result-area{
  background: linear-gradient(90deg, #00C9FF 0%, #92FE9D 100%);
}
</style>
