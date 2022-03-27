<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && Math.round(weather.main.temp - 273) > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          placeholder="Search..."
          class="search-bar"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp - 273) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
      api_key: "6736f3387fd94026524787716ac10f87",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&unitc=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      this.query = ""
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        " Januar ",
        " February ",
        " March ",
        " April ",
        " May ",
        " June ",
        " July ",
        " August ",
        " September ",
        " October ",
        " November ",
        " December ",
      ];
      let days = [
        " Sunday ",
        " Monday ",
        " Tuesday ",
        " Wendesday ",
        " Thursday ",
        " Friday ",
        " Saturday ",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}${date}${month}${year}`;
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
}
#app {
  background-image: url("./assets/cold.jpg");
  background-size: cover;
  background-position: center;
  transition: 0.4s;
  padding: 0;
  margin: 0;
}
#app .warm {
  background-image: url("./assets/warm.jpg");
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
.search-bar {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  width: 100%;
  display: block;
  padding: 15px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgb(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  background-color: rgb(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-family: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgb(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgb(0, 0, 0, 0.25);
}
.weather-box .weather {
  font-size: 48px;
  color: white;
  font-weight: 700;
  font-family: initial;
  text-shadow: 3px 6px rgb(0, 0, 0, 0.25);
}
</style>
