<template>
    <div class="weather">
      <h1>Wellcome to weather app!</h1>
      <input class="input-default" type="text" v-model="city" @keyup.enter="weather" placeholder="Weather in your city">
      <button class="btn-default" @click="weather">Search from your city</button>
      <div class="get-weather">{{ getWeather }}</div>
    </div>
</template>

<script>
export default {
  name: "Weather",
  data() {
    return {
      getWeather: null,
      city: 'Chisinau',
    };
  },
  methods: {
    async weather() {
      const URL =
        `http://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&lang=en&appid=${process.env.VUE_APP_ROOT_URL}`;
      this.getWeather = await this.axios
        .get(URL)
        .then((response) => {
          const {name, main, weather, visibility, sys, coord, wind, timezone} = response.data
          return `${new Date().toLocaleDateString()} ${new Date().toLocaleTimeString()} In ${name} ${main.temp}°C
          Feels like ${main.feels_like}°C
          ${weather[0].main} -
          ${weather[0].description}
          humidity - ${main.humidity}%
          visibility - ${visibility / 1000}km
          country - ${sys.country}
          coordinates - longitude ${coord.lon} latitude ${coord.lat}
          wind speed - ${wind.speed}m/s
          timezone - ${timezone}`
        });
    },
  },
};
</script>

<style scoped lang="scss">
@mixin reset-list {
  padding: 7px;
  border-radius: 4px;
  outline: none;
}

.weather {
  margin-top: 100px;

  .input-default {
    @include reset-list;
    border-color: #42b983;
    color: #42b983;
    font-weight: 700;
  }
  .btn-default {
    margin-left: 5px;
    color: #fff;
    background: #42b983;
    border: 2px solid #33ad77;
    cursor: pointer;
    @include reset-list;

    &:hover {
      color: #42b983;
      background: #fff;
    }
  }
  .get-weather {
    display: flex;
    justify-content: center;
    color: darkred;
    font-weight: 700;
    margin-top: 20px;
  }
}
</style>
