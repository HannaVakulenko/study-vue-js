<script>
  import axios from 'axios'
  export default {
    data() {
      return {
        city: "",
        error: "",
        info: null
      }
    },
    computed: {
      cityName() {
        return "«" + this.city + "»"
      },
      showTemp() {
        return "Temperature: " + this.info.main.temp
      },
      showFeelsLike() {
        return "Feels like: " + this.info.main.feels_like
      },
      showMinTemp() {
        return "Man temperature: " + this.info.main.temp_min
      },
      showMaxTemp() {
        return "Max temperature: " + this.info.main.temp_max
      }
    },
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = "A name of more than one character is required :)"
          return false
        }

        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=e190bd9fd2987164f5bccc14baca58a6`)
          .then(res => (this.info = res.data))
      }
    }

  }

</script>

<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>
      Find out the weather in {{ city == "" ? "your region" : cityName }}
    </p>
    <input type="text" v-model="city" placeholder="Insert city name">
    <button v-if="city != ''" @click="getWeather()">Get weather</button>
    <button disabled v-else>Enter the name of the city</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p >{{ showTemp }}</p>
      <p >{{ showFeelsLike }}</p>
      <p >{{ showMinTemp }}</p>
      <p >{{ showMaxTemp }}</p>
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
    border-radius: 50px;
    padding: 20px;
    background: #1f0f21;
    text-align: center;
    color: #fff;
  }

  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 20px;
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
  }

  .wrapper input:focus {
    border-bottom-color: #6e2d7d;
  }

  

    .wrapper button {
    background: radial-gradient(circle, rgba(238,174,202,1) 0%, rgba(148,187,233,1) 100%);
    color: #fff;
    border-radius: 10px;
    border: 2px solid rgba(238,174,202,1);
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }
  .wrapper button:disabled {
    background: grey;
    cursor: not-allowed;
    transform: none;
    border: none;

  }
  .wrapper button:disabled:hover {
    transform: none;
  }
  .wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
  }
  
</style>
