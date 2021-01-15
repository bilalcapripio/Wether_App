<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div class="container my-5">
      <h1 class="title text-center">Weather App</h1>
            <span id="histry">{{ moment("2021-1-12 8:00:00").fromNow() }}</span>
        <form class="search-location" v-on:submit.prevent="getWeather">
            <input type="text" class="form-control text-muted form-rounded p-4 shadow-sm"   placeholder="Search City..."  v-model="citySearch"   autocomplete="off" />
        </form>
      <p class="text-center my-5" v-if="cityFound" id="found">
        There's no place like <br />
        127.0.0.1
      </p>
      <div class="card rounded my-1 shadow-lg back-card overflow-hidden" v-if="visible">
        <!-- weather animation container -->
        <div>
          <div icon="sunny" v-if="clearSky"><span class="sun"></span></div>

          <!-- Animation code Start here -->
          <!-- <div icon="snowy" v-if="snowy">
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
                  </div> -->

          <!-- <div icon="stormy" v-if="stormy">
                    <span class="cloud"></span>
                    <ul>
                      <li></li>
                      <li></li>
                      <li></li>
                      <li></li>
                      <li></li>
                    </ul>
                  </div> -->

          <!-- <div icon="cloudy" v-if="cloudy">
                    <span class="cloud"></span>
                    <span class="cloud"></span>
                  </div>
                  <div icon="nightmoon" v-if="clearNight">
                    <span class="moon"></span>
                    <span class="meteor"></span>
                  </div> -->
          <!-- Animation code End here -->
        </div>

        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-">
            <p>{{ weather.cityName }} , {{ weather.country }}</p>
            <p class="todo">
              <img
                :src="weather.url + weather.icon + '.png'"
                alt="img not found"
              />
            </p>
            <span class="date">{{ dateSec() }}</span>
            <!-- <p class="">{{ weather.country }}</p> -->
          </div>
        </div>

        <div class="card-body">
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }} ° c</span>
                <p class="my-5" id="cloud">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <img src="./assets/down.svg" alt="" />
                  {{ weather.lowTemp }} ° c
                </p>
                <p>
                  <img src="./assets/up.svg" alt="" />
                  {{ weather.highTemp }} ° c
                </p>
              </div>
            </div>
          </div>

          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>{{ weather.feelsLike }} ° c</p>
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
              <h4>Friday</h4>
              <p>
                <i class="ico"
                  >{{ weather.temperature }}° / {{ weather.temperature }}°</i
                >
              </p>
              <p class="fore">
                <img
                  :src="weather.url + weather.icon + '.png'"
                  alt="img not found"
                />
              </p>
            </div>
            <div class="second">
              <h4>Saturday</h4>
              <p>
                <i class="ico"
                  >{{ weather.secondhigh }}° / {{ weather.secondlow }}°</i
                >
              </p>
              <p class="fore">
                <img
                  :src="weather.url + weather.secondicon + '.png'"
                  alt="img not found"
                />
              </p>
            </div>
            <div class="third">
              <h4>Sunday</h4>
              <p>
                <i class="ico"
                  >{{ weather.thirdhigh }}° / {{ weather.thirdlow }}°</i
                >
              </p>
              <p class="fore">
                <img
                  :src="weather.url + weather.thirdicon + '.png'"
                  alt="img not found"
                />
              </p>
            </div>
            <div class="fourth">
              <h4>Monday</h4>
              <p>
                <i class="ico"
                  >{{ weather.fourthhigh }}° / {{ weather.fourthlow }}°</i
                >
              </p>
              <p class="fore">
                <img
                  :src="weather.url + weather.fourthicon + '.png'"
                  alt="img not found"
                />
              </p>
            </div>
            <div class="fifth">
              <h4>Tuesday</h4>
              <p>
                <i class="ico"
                  >{{ weather.fifthhigh }}° / {{ weather.fifthlow }}°</i
                >
              </p>
              <p class="fore">
                <img
                  :src="weather.url + weather.fifthicon + '.png'"
                  alt="img not found"
                />
              </p>
            </div>
            <div class="sixth">
              <h4>Wednesday</h4>
              <p>
                <i class="ico"
                  >{{ weather.sixthhigh }}° / {{ weather.sixthlow }}°</i
                >
              </p>
              <p class="fore">
                <img
                  :src="weather.url + weather.sixthicon + '.png'"
                  alt="img not found"
                />
              </p>
            </div>
          </div>
          <!-- End forecast -->
        </div>
      </div>
    </div>

    <!--Geolocation Start-->
  
    <!--Geolocation End-->
  </div>
</template>
<script>
/* Start Script*/
var moment = require("moment");

//  Start All Component is here
// import citysearch from './components/citysearch.vue'
//  End All Component is here


export default {
  // components:{
  //   citysearch

  // },
  data() {
    // const dt_txt=moment();
    // console.log(weather.dt_txt.format('dddd'));
    // testing Code end

    return {
      cityFound: false,
      visible: false,
      stormy: false,
      cloudy: false,
      clearSky: false,
      clearNight: false,
      snowy: false,
      moment: moment,
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
        url: "http://openweathermap.org/img/wn/",

        //icon defined
        icon: "",
        secondicon: "",
        thirdicon: "",
        fourthicon: "",
        fifthicon: "",
        sixthicon: "",

        //min and max temp defined
        secondhigh: "",
        secondlow: "",
        thirdhigh: "",
        thirdlow: "",
        fourthhigh: "",
        fourthlow: "",
        fifthhigh: "",
        fifthhlow: "",
        sixthhigh: "",
        sixthlow: ""
      }
    };
  },
  methods: {
    getWeather: async function() {
      console.log(this.citySearch);
      // api Key
      var key = "e458609984d4efd72b9d31134bdab8eb";
      // api Link
      var baseURL = `http://api.openweathermap.org/data/2.5/forecast?q=${this.citySearch}&appid=${key}&units=metric`;
      try {
        var response = await fetch(baseURL);
        var data = await response.json();

        //testing
        console.log(data);

        //Get Data
        this.citySearch = "";
        this.weather.cityName = data.city.name;
        this.weather.country = data.city.country;
        this.weather.temperature = Math.round(data.list[0].main.temp);
        this.weather.description = data.list[0].weather[0].description;
        this.weather.lowTemp = Math.round(data.list[0].main.temp_min);
        this.weather.highTemp = Math.round(data.list[0].main.temp_max);
        this.weather.feelsLike = Math.round(data.list[0].main.feels_like);
        this.weather.humidity = Math.round(data.list[0].main.humidity);

        // Week Forecast
        this.weather.icon = data.list[0].weather[0].icon;
        this.weather.secondicon = data.list[5].weather[0].icon;
        this.weather.thirdicon = data.list[13].weather[0].icon;
        this.weather.fourthicon = data.list[21].weather[0].icon;
        this.weather.fifthicon = data.list[29].weather[0].icon;
        this.weather.sixthicon = data.list[37].weather[0].icon;

        //min and max
        this.weather.secondhigh = Math.round(data.list[5].main.temp_max);
        this.weather.secondlow = Math.round(data.list[5].main.temp_min);

        this.weather.thirdhigh = Math.round(data.list[13].main.temp_max);
        this.weather.thirdlow = Math.round(data.list[13].main.temp_min);

        this.weather.fourthhigh = Math.round(data.list[21].main.temp_max);
        this.weather.fourthlow = Math.round(data.list[21].main.temp_min);

        this.weather.fifthhigh = Math.round(data.list[29].main.temp_max);
        this.weather.fifthlow = Math.round(data.list[29].main.temp_min);

        this.weather.sixthhigh = Math.round(data.list[37].main.temp_max);
        this.weather.sixthlow = Math.round(data.list[37].main.temp_min);

        // icon
        var timeOfDay = data.list[0].weather[0].icon;

        // testing
        console.log(data.list[37].weather[0].icon);

        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }

        // Start Animation
        var mainWeather = data.list[0].weather[0].main;
        // if (mainWeather.includes("Clouds")) {
        //   this.stormy = false;
        //   this.cloudy = true;
        //   this.clearSky = false;
        //   this.clearNight = false;
        //   this.snowy = false;
        // }
        // if (mainWeather.includes("Clouds")) {
        //   this.stormy = false;
        //   this.cloudy = true;
        //   this.clearSky = false;
        //   this.clearNight = false;
        //   this.snowy = false;
        // }
        // if (mainWeather.includes("Thunderstorm") || mainWeather.includes("Rain")) {
        //   this.stormy = true;
        //   this.cloudy = false;
        //   this.clearSky = false;
        //   this.clearNight = false;
        //   this.snowy = false;
        // }
        // if (mainWeather.includes("Clear") && this.isDay) {
        //   this.stormy = false;
        //   this.cloudy = false;
        //   this.clearSky = true;
        //   this.clearNight = false;
        //   this.snowy = false;
        // }
        // if (mainWeather.includes("Clouds") && !this.isDay) {
        //   this.stormy = false;
        //   this.cloudy = false;
        //   this.clearSky = false;
        //   this.clearNight = true;
        //   this.snowy = false;
        // }
        if (mainWeather.includes("Snow")) {
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = true;
        }

        this.visible = true;
        this.cityFound = false;
      } catch (error) {
        console.log(error);
        this.cityFound = true;
        this.visible = false;
      }
    },

    // Start date timing and week
    dateSec() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
};
/* End Script*/
</script>
<style scoped>
@import "./assets/animation.css";
@import "./assets/custom.css";
#found {
  font-size: 4rem;
  color: rgb(163, 108, 108);
  position: relative;
  top: 123px;
  animation: wobble 2s alternate infinite;
}
@keyframes wobble {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(10px);
  }
  100% {
    transform: translateY(0px);
  }
}
#main h1 {
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
  letter-spacing: 5px;
  margin-bottom: 30px;
}
#main h1::first-letter {
  font-size: 5rem;
  color: #fff;
  font-family: initial;
  background: rgb(194, 53, 53);
  padding: 10px;
  border-radius: 50%;
}
.forecast {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 40px;
  background: rgb(194, 53, 53);
  z-index: 100000;
}
.forecast > div {
  margin: 40px;
  text-align: center;
}
.card-body {
  padding: 0rem;
}
.fore {
  background: #fff;
  border-radius: 10px;
}
.todo {
  position: relative;
  left: -131px;
  top: 63px;
  transform: scale(3);
}
#cloud {
  position: relative;
  top: 2px;
}
</style>
