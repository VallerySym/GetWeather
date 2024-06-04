<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == '' ? 'вашем городе' : cityName }}</p>
    <div class="container">
      <input type="text" v-model="city" name="city" id="city" placeholder="Введите город">
      <button v-if="city != ''" @click="gerWeather()">Получить погоду</button>
      <button v-else  disabled>Введите название города</button>
    </div>
    <p class="error">{{ error }}</p>
    
    <div v-if="info != null" class="info">
      <p >{{ showTemp}}</p>
      <p >{{ showFeelsLike}}</p>
      <p >{{ showMinTemp}}</p>
      <p >{{ showMaxTemp}}</p>
    </div>
 

  </div>
</template>

<script>
import axios from 'axios';

export default {

  data() {
    return {
      city: '',
      error: '',
      info: null,
    }
  },

  computed: {
    cityName() {
      return '"' + this.city + '"'
    },

    showTemp(){
      return "Температура " + Math.round(this.info.main.temp) + " °C"
    },

    showFeelsLike(){
      return "Ощущается как " + Math.round(this.info.main.feels_like) + " °C"
    },

    showMinTemp(){
      return "Минимальная температура " + Math.round(this.info.main.temp_min) + " °C"
    },

    showMaxTemp(){
      return "Максимальная температура " + Math.round(this.info.main.temp_max) + " °C"
    },
  },

  methods: {
    gerWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Название должно быть больше одного символа"

        return false
      }
      this.error = ''


      axios.get (`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=2a21ce502bef528c885bca5225ca4229`)
      .then(response=>(this.info = response.data))

      this.info =''
    }
  }
}

</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background-color: #9b72d218;
  padding: 50px;
  text-align: center;
  color: #ffffff;
  box-shadow: 0px 0px 30px -9px rgba(135,125,135,1);
}

.wrapper p {
  margin-bottom: 30px;
}

.container {
  display: flex;
  flex-direction: row;
  margin: 0 auto;
  margin-bottom: 20px;
}

.container input {
  background: transparent;
  border: none;
  border-bottom: 2px solid #523874;
  color: #fcfcfc;
  font-size: 14px;
  padding: 10px 15px;
  outline: none;
}

.container input::placeholder {
  color: #fcfcfc;
}

.container input:focus {
  border-bottom: 2px solid #b18fb7;
}

.container button {
  padding: 10px 20px;
  border-radius: 10px;
  background-color: #b18fb7;
  border: none;
  color: #fcfcfc;
  cursor: pointer;
  transition: transform 500ms ease;
  margin-left: 20px;
}

.container button:hover {
  transform: scale(1.1) translateY(-3px);
}

.container button:disabled {
  background-color: #8d7292;
  pointer-events: none;
}

.error{
  color:#be2e24 ;
}

.info p {
  margin-bottom: 10px;
}

</style>