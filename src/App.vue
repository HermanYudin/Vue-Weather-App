<template>
  <div class="wrapper" id="app">
    <h1>Погода</h1>
    <p>
      Узнайте погоду в
      {{ city == "" ? "вашем городе :)" : cityName }}
    </p>
    <input
      type="text"
      v-model="city"
      onkeyup="this.value = this.value.replace(/\d/g,'')"
      v-on:keyup.enter="getWeather()"
      placeholder="Введите город"
    />
    <button v-if="city != ''" v-on:click="getWeather()">Узнать погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>
    <div class="temp-info" v-if="info != null">
      <p>{{ cityTemp }}</p>
      <p>{{ cityTempFeelsLike }}</p>
      <p>{{ cityMinTemp }}</p>
      <p>{{ cityMaxTemp }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Введите полное название города";
        return false;
      }
      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=4d95b03d91053da5c14cb9d3203d596b`
        )
        .then((res) => (this.info = res.data));
    },
  },
  computed: {
    cityName() {
      return "«" + this.city.toUpperCase() + "»";
    },
    cityTemp() {
      return "Температура: " + this.info.main.temp;
    },
    cityTempFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like;
    },
    cityMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min;
    },
    cityMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max;
    },
  },
};
</script>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgb(238, 174, 202);
  background: radial-gradient(
    circle,
    rgba(238, 174, 202, 1) 0%,
    rgba(148, 187, 233, 1) 100%
  );
  box-shadow: 3px 3px 3px grey;
  color: white;
  text-align: center;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 10px;
  font-size: 22px;
}

.wrapper input {
  margin-top: 20px;
  border: 0;
  box-shadow: 2px 2px 2px white;
  border-radius: 10px 0px 0px 10px;
  padding-left: 10px;
  padding-right: 10px;
  outline: none;
  min-height: 30px;
  color: darkcyan;
}

.wrapper button {
  box-shadow: 2px 2px 2px white;
  background: rgb(238, 174, 202);
  background: radial-gradient(
    circle,
    rgba(238, 174, 202, 1) 0%,
    rgba(148, 187, 233, 1) 100%
  );
  border: none;
  cursor: pointer;
  min-height: 30px;
  border-radius: 0px 10px 10px 0px;
  min-width: 100px;
  padding-left: 10px;
  padding-right: 10px;
  transition: transform 500ms ease;
  transition: all 0.2s ease;
}

.wrapper button:disabled {
  background: skyblue;
}

.wrapper button:disabled:hover {
  background: skyblue;
  transition: none;
  transform: none;
}

.wrapper button:hover {
  box-shadow: 2px 2px 2px white;
  background: rgb(63, 94, 251);
  background: radial-gradient(
    circle,
    rgba(63, 94, 251, 1) 0%,
    rgba(252, 70, 107, 1) 100%
  );
  transform: scale(1.05) translateY(-1px);
}

.error {
  color: purple;
}

.temp-info {
  margin-top: 40px;
}
</style>
