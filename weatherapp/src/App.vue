<template>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;900&display=swap"
    rel="stylesheet"
  />
  <div
    id="app"
    :class="
      typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'hot' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search for a place"
          v-model="query"
          @keypress="getWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
        <div class="location-box">
          <div class="location">{{ cityName }}, {{ countryName }}</div>
          <div class="date">{{ setDate() }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{ Math.round(temp) }}°c</div>
          <div class="weather">{{ weatherType }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      apiKey: "99d6a5806ee2b635f31feec0ea1a2bfa",
      baseUrl: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      date: "",
      cityName: "",
      countryName: "",
      temp: 0,
      weatherType: "",
    };
  },

  methods: {
    getWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `${this.baseUrl}weather?q=${this.query}&units=metric&APPID=${this.apiKey}`
        )
          .then((res) => res.json())
          .then((data) => this.setResults(data));
      }
    },

    setDate() {
      const date = new Date();
      const months = [
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
        "December",
      ];
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const dayOfWeek = days[date.getDay()];
      const month = months[date.getMonth()];
      const dayOfMonth = date.getDate();
      const year = date.getFullYear();
      return `${dayOfWeek} ${dayOfMonth} ${month} ${year}`;
    },

    setResults(results) {
      this.weather = results;
      this.temp = this.weather.main.temp;
      this.cityName = this.weather.name;
      this.countryName = this.weather.sys.country;
      this.weatherType = this.weather.weather[0].main;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

#app {
  background-image: url(./assets/cold.jpg);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.hot {
  background-image: url("./assets/hot.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 20px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 25px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.8);
  color: #313131;
  box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px,
    rgba(0, 0, 0, 0.22) 0px 15px 12px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: bold;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
