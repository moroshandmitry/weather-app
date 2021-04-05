<template>
    <div class="weather">
      <h1>Wellcome to weather app!</h1>
      <div class="time-clock">Your local time {{ timeClock }}</div>
      <input class="input-default" type="text" v-model="city" @keyup.enter="getWeather" placeholder="Weather in your city">
      <button class="btn-default" @click="getWeather">Search from your city</button>

      <div class="weather-info" v-if="weather !== null">
        <div>In {{weather.name}} {{weather.main.temp}}°C, {{weather.weather[0].description}}
            <img :src="urlIco + `${weather.weather[0].icon}.png`" />
        </div>
        <div>Feels like {{weather.main.feels_like}}°C</div>

        <div>Found location coordinates of {{ weather.sys.country }} {{ weather.name }}
          <div>Longitude {{weather.coord.lon}}</div> 
          <div>Latitude {{weather.coord.lat}}</div>
        </div>
        
        <div>Humidity {{weather.main.humidity}}%</div>
        <div>Visibility {{weather.visibility / 1000}} km</div>
        <div>Wind speed {{weather.wind.speed}} m/s</div>
        <div>Timezone {{weather.timezone / 60 / 60}} hours</div>
      </div>
    </div>
</template>

<script>
export default {
  name: "Weather",
  data() {
    return {
      timeClock: new Date().toLocaleTimeString(),
      weather: null,
      urlIco: 'http://openweathermap.org/img/wn/',
      city: '',
    };
  },
  methods: {
    async getWeather() {
      const URL =
        `http://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&lang=en&appid=${process.env.VUE_APP_ROOT_URL}`;
        this.weather = await this.axios.get(URL).then((response) => response.data)
    },
  },
  mounted() {
    setInterval(() => this.timeClock = new Date().toLocaleTimeString(), 1000)
  },
};
</script>

<style scoped lang="scss">
$default-color: darkslategray;

@mixin input-btn {
  padding: 7px;
  border-radius: 4px;
  outline: none;
}

.weather {
  margin-top: 100px;

  .input-default {
    @include input-btn;
    border-color: #42b983;
    color: $default-color;
    font-weight: 700;
  }
  .btn-default {
    margin-left: 5px;
    color: #fff;
    background: #42b983;
    border: 2px solid #33ad77;
    cursor: pointer;
    @include input-btn;

    &:hover {
      color: #42b983;
      background: #fff;
    }
  }
  .weather-info {
    color: $default-color;
    font-weight: 700;
    margin-top: 20px;

    & div {
      padding: 5px;
    }
  }
  .time-clock {
    display: flex;
    justify-content: center;
    color: $default-color;
    font-size: 24px;
    margin: 20px 0px;
  }
}
</style>
