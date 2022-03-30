<template>
  <div id="WeatherWrap" class="weather-wrap" v-if="typeof weather.main != 'undefined'">
    <div class="location-box">
      <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
      <div class="date">
        {{ daysOfWeek[d.getDay()] }}
        {{ d.getDate() }}
        {{ monthsOftheYear[d.getMonth()] }}
        {{ d.getFullYear() }}
      </div>
    </div>
    <div class="weather-box">
      <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
      <div class="weather">{{ weather.weather[0].main }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "InitialLocation",
  async created() {
    const response = await fetch(
      `https://api.ipdata.co?api-key=f36ae1346c9d71d1074e18b12d2d3421f7b5232354390f25abadb8b7`
    );
    let res = await response.json();
    let city = res.city;
    const data = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=10a7cd4554bd2169ea1bb02e8300fea7`
    );
    let resWeather = await data.json();
    this.weather = resWeather;
  },
  props: {
    d: Object,
  },
  data() {
    return {
      location: {},
      weather: {},
      daysOfWeek: ["Sabado", "Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta"],
      monthsOftheYear: [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julho",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro",
      ],
    };
  },
};
</script>

<style>
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
