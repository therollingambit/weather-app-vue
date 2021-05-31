<template>
  <div class="app" :class="{warm: hotWeather}">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="search"
        />
      </div>
      <div class="location-box">
        <div class="location">
          {{ weather.name }}, {{ weather.sys.country }}
        </div>
        <div class="date">{{ timestamp }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ weather.main.temp.toFixed(1) }}°c</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
    </main>
  </div>
</template>

<script>
import { API_KEY } from "@/apis/api.js";
import axios from "axios";
import moment from "moment";

export default {
  name: "App",
  components: {},
  data() {
    return {
      api: { key: API_KEY, base: "https://api.openweathermap.org/data/2.5/" },
      query: "",
      weather: {},
    };
  },
  methods: {
    async search(evt) {
      if (evt.key === "Enter") {
        await axios
          .get(
            `${this.api.base}weather?q=${this.query}&units=metric&APPID=${this.api.key}`
          )
          .then((res) => {
            this.weather = res.data;
            this.query = "";
            console.log(this.weather);
          })
          .catch((err) => console.log(err));
      } else {
        console.log("error");
      }
    },
  },
  computed: {
    timestamp: function() {
      return moment(new Date()).format("dddd Do MMMM YYYY");
    },
    hotWeather: function() {
      return this.weather.main.temp > 16;
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
}

.app {
  background-image: url("~@/assets/cold-bg.jpeg");
  background-size: cover;
  transition: 0.4s ease-out;
  width: 100%;
  height: 100vh;
  background-position: bottom;
}

.app.warm {
  background-image: url("~@/assets/warm-bg.jpeg");
}

main {
  min-height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.75)
  );
  padding: 25px;

  .search-box {
    width: 100%;
    margin: 0 auto 75px;
  }

  .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    appearance: none;
    background: none;
    border: none;
    outline: none;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 16px;
    margin-top: -10px;
    box-shadow: 0 5px rgba(0, 0, 0, 0.2);
    color: #313131;
    font-size: 20px;
    transition: 0.4s ease;
  }
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box {
  .location {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
  }

  .date {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
    text-shadow: 2px 2px rgba(50, 50, 70, 0.5);
  }
}

.weather-box {
  text-align: center;

  .temp {
    position: relative;
    display: inline-block;
    margin: 30px auto;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 16px;
    padding: 15px 25px;
    color: #fff;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(50, 50, 70, 0.5);
    text-align: center;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.2);
  }

  .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
  }
}
</style>