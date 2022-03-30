<template>
  <div
    id="app"
    :class="typeof weather.main != 'undefined' && weather.main.temp > 19 ? 'warm' : ''"
  >
    <main>
      <SearchBox :fetchLocation="fetchLocation" />
      <WeatherWrap :d="d" :weather="weather" />
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
  data() {
    return {
      api_key: "10a7cd4554bd2169ea1bb02e8300fea7",
      url_base: "https://api.openweathermap.org/data/2.5/",
      weather: {},
      d: new Date(),
      errorNotFound: false,
    };
  },
  methods: {
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
        console.log(error);
      } else {
        this.setResults(response.json());
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
