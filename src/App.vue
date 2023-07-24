<script>
import axios from "axios";
export default {
  data() {
    return {
      apiKay: "1add16b803370c39713591c1c31d52af",
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "'" + this.city + "'";
    },
    showTemp() {
      return "Температура: " + Math.round(this.info.temp);
    },
    showFeelsLike() {
      return (
        "Відчувається: " + Math.round(this.info.feels_like)
      );
    },
    showMinTemp() {
      return (
        "Мінімальна: " + Math.round(this.info.temp_min)
      );
    },
    showMaxTemp() {
      return (
        "Максимальна: " + Math.round(this.info.temp_max)
      );
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Веедіть більше одного символа";
        return false;
      }
      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${this.apiKay}`
        )
        .then((res) => (this.info = res.data.main));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Додаток погоди</h1>
    <p>
      Дізнатися погоду у
      {{ city === "" ? "вашому місті" : cityName }}
    </p>
    <input
      type="text"
      v-model="this.city"
      placeholder="Введіть місто"
    />
    <button v-if="city != ''" @click.enter="getWeather()">
      Отримати погоду
    </button>
    <button disabled v-else>Введіть місто</button>
    <p class="error">{{ error }}</p>
    <div v-if="this.info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}
.wrapper {
  width: 900px;
  height: 500px;
  padding: 20px;
  text-align: center;
  border-radius: 50px;
  background: #1f0f24;
  color: #fff;
}
.wrapper h1 {
  margin-top: 50px;
}
.wrapper p {
  margin-top: 50px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  transition: border-bottom-color 500ms;
}
.wrapper input:hover,
.wrapper input:focus {
  border-bottom-color: aqua;
}
.wrapper button {
  background: #097978;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #020024;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease, background 500ms;
}
.wrapper button:hover,
.wrapper button:focus {
  background: #00d4ff;
  transform: scale(1.1) translateY(-1px);
}
.wrapper button:disabled {
  background: #042a29;
  cursor: not-allowed;
}
</style>
