<template>
  <div
    id="app"
    :class="typeof weather.main != 'undefined' && weather.main.temp > 19 ? 'warm' : ''"
  >
    <main>
      <SearchBox :fetchLocation="fetchLocation" />
      <WeatherWrap
        :d="d"
        :weather="weather"
        :emoji="emoji"
        v-if="typeof weather.main != 'undefined'"
      />
      <div class="errorNotFound" v-if="errorNotFound"><h2>Cidade Não Encontrada</h2></div>
    </main>
  </div>
</template>

<script>
import SearchBox from "./components/searchBox/SearchBox";
import WeatherWrap from "./components/weatherWrap/WeatherWrap";

export default {
  name: "App",
  components: {
    SearchBox,
    WeatherWrap,
  },
  async created() {
    const response = await fetch(`${this.ip_url}`);
    let res = await response.json();
    let city = res.city;
    this.emoji = res.emoji_flag;
    const data = await fetch(
      `${this.url_base}weather?q=${city}&units=metric&appid=${this.api_key}`
    );
    let resWeather = await data.json();
    this.weather = resWeather;
  },
  data() {
    return {
      ip_url:
        "https://api.ipdata.co?api-key=f36ae1346c9d71d1074e18b12d2d3421f7b5232354390f25abadb8b7",
      api_key: "10a7cd4554bd2169ea1bb02e8300fea7",
      url_base: "https://api.openweathermap.org/data/2.5/",
      weather: {},
      d: new Date(),
      errorNotFound: false,
      emoji: "",
    };
  },
  methods: {
    getFlagEmoji(countryCode) {
      const codePoints = countryCode
        .toUpperCase()
        .split("")
        .map((char) => 127397 + char.charCodeAt());
      return String.fromCodePoint(...codePoints);
    },
    setResults(results) {
      this.weather = results;
    },
    async fetchLocation(input) {
      const response = await fetch(
        `${this.url_base}weather?q=${input}&units=metric&appid=${this.api_key}`
      );
      if (response.ok === false) {
        this.errorNotFound = true;
        this.weather = {};
      } else {
        let res = await response.json();
        this.setResults(res);
        this.emoji = this.getFlagEmoji(res.sys.country);
        this.errorNotFound = false;
      }
      return response;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", "sans-serif";
}
#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 400ms;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.errorNotFound {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
}
.errorNotFound h2 {
  animation: fadein 400ms;
  text-align: center;
  color: #fff;
  font-size: 35px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
@keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
