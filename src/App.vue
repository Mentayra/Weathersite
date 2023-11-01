<script>
import axios from "axios"
export default {
data() {
  return {
    city: "",
    err:"",
    info:null
    }
  },
  computed: {
    cityName(){
      return this.city 
    },
    Temp(){
      return "Температура:" + this.info.main.temp + "°"
    },
    FeelsLike(){
      return "Ощущается как:" + this.info.main.feels_like + "°"
    },
    MinTemp(){
      return "Минимальная температура сегодня:" + this.info.main.temp_min + "°"
    },
    MaxTemp(){
      return "Максимальная температура сегодня:" + this.info.main.temp_max + "°"
    },
  },
  methods: {
    getWeather(){
      if (this.city.trim().length < 2) {
        this.err = "Нужно название более одного символа :)"
        return false
      }
      this.err = ""

      axios.get
      (`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
        .then(res => (this.info = res.data))
    }
  },
}
</script>

<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнаю погоду в городе 
        {{city == "" ? "каком угодно" : cityName}}
        </p>
        <input
        type="text"
        v-model="city"
        placeholder="Введите город"/>
        <button v-if="city != ''" @click="getWeather()">Узнать погоду</button>
        <button disabled v-else>Введите назавание города !</button>
        <p class="err ">{{ err }}</p>
        <div v-if="info != null">
          <h2>Город:  {{ cityName}}</h2>
          <br/>
          <h3>{{ Temp }}</h3>
          <br/>
          <h3>{{ FeelsLike }}</h3>
          <br/>
          <h3>{{ MinTemp }}</h3>
          <br/>
          <h3>{{ MaxTemp }}</h3>
        </div>
        
    </div>
</template>

<style scoped>
.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 70px;
  background-color: #1f0f24;
  padding: 20px;
  text-align: center;
  color: rgb(255, 255, 255);
}

.err{
color: #b91111;
font-size: 23px ;
}
.wrapper h1{
  margin-top: 50px;
}

.wrapper p{
  margin-top: 20px;
}

.wrapper input{
  margin-top: 30px;
  background-color: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus{
  border-bottom-color: #d1d1d1;
}

.wrapper button:disabled{
  background-color: #a40c0c;
  border: 3px solid #f22424e7;
  cursor: not-allowed;
}
.wrapper button{
  background-color: #e3bc3b;
  color: #fff;
  border-radius: 10px;
  border: 3px solid #b99829;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 300ms ease;
}

.wrapper button:hover{
  transform: scale(1.1) translateY(-5px);
}
</style>
