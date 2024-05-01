<script setup>
import { ref } from 'vue'

const apiKey = ref('4755ef89428c474e131b129e83047c8c');
const apiUrl = ref('https://api.openweathermap.org/data/2.5/weather?&units=metric&q=');
const location = ref('Paris'); // Default location set to Paris
const newItem =()=>{
  checkWeather()
  location.value = ''
}

async function checkWeather() {
  try {
    const response = await fetch(`${apiUrl.value}${location.value}&appid=${apiKey.value}`);
    const data = await response.json();
    console.log(data);
    document.querySelector('h1').innerHTML = Math.round(data.main.temp)+'°C';
    document.querySelector('h2').innerHTML = data.name;
    document.querySelector('h3').innerHTML = Math.round(data.wind.speed)+' Km/h';
    document.querySelector('h4').innerHTML = data.main.humidity +'%';
  } catch (error) {
    console.error('Error fetching weather data:', error);
  }
}

// Call checkWeather initially
checkWeather();

</script>

<template>
  <div class="w_app">

    <div class="search-place">
      <div class="input">
        <input 
        @submit.prevent="checkWeather()"
        v-model="location" 
        @keyup.enter="newItem" 
        placeholder=" Type city name" 
        size="300"></div> 

        <button @click="newItem">
          <img src="..//img/search_icon.png" alt="">
        </button>

      </div>
    <div class="weather">
      <img src="..//img/Sun.png" alt="">
      <div class="temp">        
        <h1></h1>
        <h2></h2>
      </div>
    </div>

    <div class="details">
      <div class="wind">
        <img src="..//img/wind.png" alt="">
        <h3></h3>
      </div>
      <div class="humidity">
        <img src="..//img/humidity.png" alt="">
        <h4></h4>
      </div>
    </div>
  </div>
</template>

<style scoped>

*{
  margin: 0 auto;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  box-sizing: border-box;
  }

body{
  color: #222;
  margin: 0 auto;
  background-color: #222;
}


.w_app{
  padding: 15px;
  width: 550px;
  height: auto;
  border-radius: 25px;
  background:linear-gradient(150deg, #cfd9df,#e2ebf0,#b9b9b9);
}

.search-place {
  width: 100%;
  height: 50px;
  display: flex;
  align-items: center;
  margin: 0px auto 15px auto;
}

.input {
  width:100%;
  min-width: 300px;
  height: 50px;
  background: #ffffff;
  color: #555;
  font-size: 18px;
  border-radius: 25px;
  display: flex;
  align-items: center;
  }

  .input input{
    width:94%;
    border: 0;
    outline: 0;
    background: #ffffff;
    color: #555;
    height: 40px;
    margin-left: 15px;
    font-size: 18px;
  }

.search-place button {
  width: 20%;
  height: 50px;
  margin-left: 10px;
  border-radius: 25px;
  border: 0;
  outline: 0;
  background: #ffffff;
  color: #555;
}

.search-place button img {
  width: 25px;

}


.weather {
  background: #ffffff;
  border-radius: 25px;
  width: 100%;
  margin: auto;
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 10px;
}

.weather img {
  width: 150px;
  height: fit-content;
}

.temp {
  width: auto;
  height: auto;
}

.details {
  background: #ffffff;
  border-radius: 25px;
  width: 100%;
  margin:15px auto 15px auto;
  display: flex;
  justify-content: space-between;
}

.wind, .humidity{
  margin: 0 10px 0 10px;
  width: 200px;
  display: flex;
  align-items: center;
  font-size: 20px;
}

.wind img {
  width: 50px;
}
.humidity img {
  width: 50px;
}
</style>
