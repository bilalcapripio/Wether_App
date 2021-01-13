<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div class="container my-5">
      <h1 class="title text-center">Weather App</h1>
      <span id="histry">{{moment('2021-1-12 8:00:00').fromNow()}}</span>
      <form class="search-location" v-on:submit.prevent="getWeather">
        <input
          type="text"
          class="form-control text-muted form-rounded p-4 shadow-sm"
          placeholder="Search City..."
          v-model="citySearch"
          autocomplete="off"
        />
      </form>
      <p class="text-center my-5" v-if="cityFound" id="found">There's no place like <br> 127.0.0.1</p>
      <div
        class="card rounded my-1 shadow-lg back-card overflow-hidden" v-if="visible">
        <!-- weather animation container -->
        <div>
          <div icon="sunny" v-if="clearSky">
            <span class="sun"></span>
          </div>

          <div icon="snowy" v-if="snowy">
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>

          <div icon="stormy" v-if="stormy">
            <span class="cloud"></span>
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>
          <div icon="cloudy" v-if="cloudy">
            <span class="cloud"></span>
            <span class="cloud"></span>
          </div>
          <div icon="nightmoon" v-if="clearNight">
            <span class="moon"></span>
            <span class="meteor"></span>
          </div>
        </div>

        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p>{{ weather.cityName }}</p>
          <span class="date">{{ dateSec() }}</span>
            <p class="">{{ weather.country }}</p>
          </div>
        </div>

        <div class="card-body">
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-4">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <img src="./assets/down.svg" alt="" />
                  {{ weather.lowTemp }}&deg;C
                </p>
                <p>
                  <img src="./assets/up.svg" alt="" />
                  {{ weather.highTemp }}&deg;C
                </p>
              </div>
            </div>
          </div>

          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>{{ weather.feelsLike }}&deg;C</p>
              <span>Feels like</span>
            </div>
            <div class="col text-center">
              <p>{{ weather.humidity }}%</p>
              <span>humidity</span>
            </div>
          </div>
            <!-- Start forecast -->
            <div class="forecast">
              <div class="first">
                <h4>Sunday</h4>
                <p><i class="ico">{{weather.temperature}} ° c</i></p>
                <p class="fore"><img width="50px" height="50px" v-for-key="img in images" :src="img" alt="img not found"></p>
              </div>
                <div class="second">
                <h4>Monday</h4>
                <p><i class="ico">{{weather.temperature}} ° c</i></p>
                <p class="fore"><span>&#9788;</span> / <span>&#9788;</span></p>
              </div>
                <div class="third">
                <h4>Tuesday</h4>
                <p><i class="ico">{{weather.temperature}} ° c</i></p>
                <p class="fore"><span>&#9788;</span> / <span>&#9788;</span></p>
              </div>
                <div class="fourth">
                <h4>Wednesday</h4>
                <p><i class="ico">{{weather.temperature}} ° c</i></p>
                <p class="fore"><span>&#9788;</span> / <span>&#9788;</span></p>
              </div>
                <div class="fifth">
                <h4>Thursday</h4>
                <p><i class="ico">{{weather.temperature}} ° c</i></p>
                <p class="fore"><span>&#9788;</span> / <span>&#9788;</span></p>
              </div>
                <div class="sixth">
                <h4>Friday</h4>
                <p><i class="ico">{{weather.temperature}} ° c</i></p>
                <p class="fore"><span>&#9788;</span> / <span>&#9788;</span></p>
              </div>
                <div class="seventh">
                <h4>Saturday</h4>
                <p><i class="ico">{{weather.temperature}} ° c</i></p>
                <p class="fore"><span>&#9788;</span> / <span>&#9788;</span></p>
              </div>
            </div>
            <!-- End forecast -->

        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    var moment = require('moment');
    // const dt_txt=moment();
    // console.log(weather.dt_txt.format('dddd'));
    return {
      cityFound: false,
      visible: false,
      stormy: false,
      cloudy: false,
      clearSky: false,
      clearNight: false,
      snowy: false,
      moment:moment,
      isDay: true,
      citySearch: "",
      weather: {
        cityName: "Karachi",
        country: "PK",
        temperature: 12,
        description: "Clouds everywhere",
        lowTemp: "19",
        highTemp: "30",
        feelsLike: "20",
        humidity: "55",
        dt_txt: "",
        images:['https://source.unsplash.com'],

      },
    };
  },
  methods: {
    getWeather: async function () {
      console.log(this.citySearch);
      var key = "4aa3725a391e77d79c3a15a904a3666b";
      var baseURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
      try {
        var response = await fetch(baseURL);
        var data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);
        this.weather.icon = data.weather[0].icon;
        var timeOfDay = data.weather[0].icon;

        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }

        var mainWeather = data.weather[0].main;
        if (mainWeather.includes("Clouds")) {
          this.stormy = false;
          this.cloudy = true;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clouds")) {
          this.stormy = false;
          this.cloudy = true;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Thunderstorm") || mainWeather.includes("Rain")) {
          this.stormy = true;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clear") && this.isDay) {
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = true;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clouds") && !this.isDay) {
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = true;
          this.snowy = false;
        }
        if (mainWeather.includes("Snow")) {
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = true;
        }

        this.visible = true;
        this.cityFound = false;
      } catch (error) {console.log(error);
        this.cityFound = true;
        this.visible = false;
      }
    },
    dateSec () {
          let d = new Date();
          let months=["January","February","March","April","May","June","July","August","September","October","November","December"];
          let days=["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];

          let day = days[d.getDay()];
          let date = d.getDate();
          let month = months[d.getMonth()];
          let year = d.getFullYear();
          return  `${day} ${date} ${month} ${year}`;

    },
  }
    
};
</script>

<style scoped>
@import "./assets/animation.css";
@import "./assets/custom.css";
#found{
    font-size: 4rem;
    color: rgb(163, 108, 108);
    position: relative;
    top: 123px;
    animation: wobble 2s alternate infinite;
}
@keyframes wobble{
  0%{  
    transform: translateY(0px);
}
  50%{  
    transform: translateY(10px);
}
  100%{  
    transform: translateY(0px);
}
}
#main h1{
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  letter-spacing: 5px;
  margin-bottom: 30px;

}
#main h1::first-letter{
  font-size: 5rem;
  color: #fff;
  font-family: initial;
  background:rgb(194, 53, 53);
  padding: 10px;
  border-radius: 50%;
}
.forecast{
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 40px;
  background:rgb(194, 53, 53);
  z-index: 100000;
}
.forecast > div{
margin: 30px;
text-align: center;
}
.card-body {

    padding: 0rem;
}
</styl