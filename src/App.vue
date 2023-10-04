<script setup>

</script>
<template>
  <div id="app" :class="weather.temp>16?'warn':''">
    <main>
      <div  class="search-box">
        <input type="text" @keydown="fetchWeather" v-model="query" class="search-bar" placeholder="search...">
      </div>
      <div v-if="weather.name!=''" class="weather-wrap">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}
          </div>
          <div class="date">
            {{ dateBuilder() }}
          </div>
        </div>
        <div class="weather-box">
          <div class="temp">
            {{ weather.temp }} C
          </div>
          <div>
            <img :src="weather.icon" alt="">
          </div>
          <div class="weather">
            {{ weather.text }}
          </div>
        </div>
      </div>
      <div v-else class="weather-wrap">
        <div class="location-box">
          <div class="location">
            Types City Name On Search Bar..
          </div>
          <div class="date">
            {{ dateBuilder() }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
<script >
export default {
  name: "app",
  data() {
    return {
      api_key: '391654ed9b0a44928b580604230410',
      url_base: 'http://api.weatherapi.com',
      query: '',
      weather: {
        name: "",
        temp: "",
        text: "",
        icon:""
      }
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}/v1/current.json?key=${this.api_key}&q=${this.query}&aqi=no
`)
          .then(response => {
            console.log(response)
            return response.json();  // This returns a promise
          })
          .then(data => {
            console.log('JSON Data:', data);
            // Use the data as needed here
            this.setResults(data)
          })
      }

    },
    setResults(results) {
      this.weather.name = results.location.name;
      this.weather.temp = results.current.temp_c;
      this.weather.text = results.current.condition.text;
      this.weather.icon = results.current.condition.icon;
    }, dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }


  }
}
</script>
<style scoped>
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}

body {
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif
}

#app {
  background-size: cover;
  background-position: top;
  background-image: url(assets/cloudy.jpg);
  transition: 0.4s;
}

main {
  height: 100vh;
  padding: 15px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.15), rgba(0, 0, 0, 0.75));
}

.search-box .search-bar {
  width: 100%;
  padding: 15px;
  display: block;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.35);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.weather-wrap {
  margin-top: 32px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500px;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #ffffff9b;
  font-size: 20px;
  font-style: italic;
  text-align: center;
  margin-top: 20px;
  font-weight: 100;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  color: #fff;
  font-size: 102px;
  font-weight: 600;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.35);
  display: inline-block;
  padding: 8px 16px;
  margin: 30px 0px;
  border-radius: 16px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
#app.warn{
  background-image: url(assets/weather.jpg);
}
#app.rain{
  background-image: url(assets/rainy.jpg);
}
</style>
