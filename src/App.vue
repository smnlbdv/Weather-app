<template>

  <div className="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в <span className="sity-span">{{ city == "" ? "Вашем городе" : city }}</span></p>

    <div class="input-block">
      <input type="text" v-model="city" @keydown="pressInput($event)" placeholder="Введите свой город...">
      <button @click="getWeather()">Получить погоду</button>
      <p class="error">{{ error }}</p>
    </div>

    <div class="values-list" v-if="info != null">
        <WeatherItem :title="'Температура'" :value="info.temp + ' °C'" :link="'./images/icon/thermometer.svg'"  :isLoaded="isLoaded" :setLoaded="setLoaded"/>
        <WeatherItem :title="'Ощущается как'" :value="info.feels_like + ' °C'" :link="'./images/icon/hand.svg'" :isLoaded="isLoaded" :setLoaded="setLoaded"/>
        <WeatherItem :title="'Макс. температура'" :value="info.temp_max + ' °C'" :link="'./images/icon/increasing.svg'" :isLoaded="isLoaded" :setLoaded="setLoaded"/>
        <WeatherItem :title="'Мин. температура'" :value="info.temp_min + ' °C'" :link="'./images/icon/decreasing.svg'" :isLoaded="isLoaded" :setLoaded="setLoaded"/>
        <WeatherItem :title="'Влажность воздуха'" :value="info.humidity + ' %'" :link="'./images/icon/droplet.svg'" :isLoaded="isLoaded" :setLoaded="setLoaded"/>
        <WeatherItem :title="'Давление'" :value="info.pressure+ ' Па'" :link="'./images/icon/anatomical-heart.svg'" :isLoaded="isLoaded" :setLoaded="setLoaded"/>
    </div>


  </div>


</template>

<script>
import axios from "axios"
import WeatherItem from "./components/weather-item/WeatherItem.vue";
import Cookies from 'js-cookie';

export default {
  components: {
    WeatherItem,
  },
  mounted() {
    if (Cookies.get('city') !== '') {
      this.city = Cookies.get('city');
      this.getWeather();
    }
  },
  data() {
    return {
      city: '',
      error: '',
      info: null,
      isLoaded: false
    }
  },
  methods: {
    getWeather() {
      if(this.city) {
        this.setLoaded(false)

        if(this.city.trim().length <= 3) {
          this.error = "Нужно название более 3 символов"
          this.info = null
          return false
        }

        this.error = ''

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=0ccfe09cfacfd5161654a192d2f6bcad`)
          .then(res => {
            this.info = res.data.main
            Cookies.set('city', this.city, { expires: 3 });
          })
      }
    
    },
    pressInput(event) {      
      if(event.key == "Enter") {
        this.getWeather()
      }
    },
    setLoaded(bool) {
      this.isLoaded = bool
    }
  },
}

</script>

<style lang="scss" scoped src="./app.scss"></style>
