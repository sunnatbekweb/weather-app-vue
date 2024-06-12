<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  data() {
    return {
      city: "" as string,
      error: "" as string,
      info: null as any | null,
    };
  },
  computed: {
    cityName(): string {
      return "<<" + this.city + ">>";
    },
  },
  methods: {
    async getWeather() {
      if (this.city.trim().length < 3) {
        this.error = "You need to enter more letter!";
        return false;
      } else {
        this.error = "";
        this.info = null;
        try {
          const res = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=f0f6e59dd9133eeb6c2d26a064b9d9ef`
          );
          this.info = res;
        } catch {
          this.error = `Cannot get weather in ${this.city}`;
        }
      }
    },
  },
});
</script>

<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>Seeing weather in {{ city ? cityName : "your city" }}</p>
    <input type="text" v-model="city" placeholder="Enter city" />
    <button v-if="city != ''" @click="getWeather">Get weather</button>
    <button v-else disabled>Enter city name</button>
    <p class="error" v-if="error">{{ error }}</p>
    <div v-if="info">
      <p>{{ info.data.weather[0].main }}</p>
      <p>{{ info.data.main.temp }} ^C</p>
      <p>{{ info.data.wind.speed }} m/s</p>
      <p>{{ info.data.sys.country }}</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  background: #090018;
  border-radius: 50px;
  text-align: center;
  padding: 20px;
}

.wrapper h1 {
  font-size: 32px;
  color: #ffffff;
  margin-bottom: 10px;
}

.wrapper p {
  color: #ffffff;
  font-size: 20px;
}

.wrapper input {
  margin-top: 30px;
  background-color: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background-color: #e3bc4b;
  color: #ffffff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1);
}

.wrapper button:active {
  transform: scale(0.9);
}

.wrapper button:disabled {
  background: #e3bd4b9d;
  cursor: not-allowed;
}

.wrapper button:disabled:hover {
  transform: scale(1);
}

.wrapper .error {
  color: #d03939;
}
</style>
