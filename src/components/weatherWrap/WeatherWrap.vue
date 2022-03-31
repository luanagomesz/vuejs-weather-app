<template>
  <div id="WeatherWrap" class="weather-wrap" v-if="typeof weather.main != 'undefined'">
    <div class="location-box">
      <div class="location">
        {{ weather.name }}, {{ weather.sys.country }} {{ emoji }}
      </div>
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
      <img :src="icon" class="icon" />
      <div class="extra-information">
        <p>Sensação de: {{ Math.round(weather.main.feels_like) }}°c</p>
        <p>
          Vento:
          {{ weather.wind.speed }} km/h
        </p>
        <p>Umidade: {{ weather.main.humidity }}%</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WeatherWrap",
  props: {
    d: Object,
    weather: Object,
    emoji: String,
  },
  watch: {
    weather: {
      immediate: true,
      handler(NewValue) {
        let res = NewValue;
        this.icon = `http://openweathermap.org/img/w/${res.weather[0].icon}.png`;
      },
    },
  },
  data() {
    return {
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
      icon: "",
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
.weather-box .extra-information {
  color: #fff;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
