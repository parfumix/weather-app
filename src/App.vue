<script setup>
import { ref } from 'vue'

const apiKey = ref('4755ef89428c474e131b129e83047c8c');
const apiUrl = ref('https://api.openweathermap.org/data/2.5/weather?&units=metric&q=');
const location = ref('Paris'); // Default location set to Paris
const newItem =()=>{
  checkWeather()
  location.value = ''
}
const temp = ref('');
const cityName = ref('');
const windSpeed = ref('');
const humidity = ref('');
const icon = ref('');
const errorMessage = ref('');

async function checkWeather() {
  try {
    const response = await fetch(`${apiUrl.value}${location.value}&appid=${apiKey.value}`);
    const data = await response.json();
    console.log(data);
    if (response.ok) {
      temp.value = Math.round(data.main.temp) + '°C';
      cityName.value = data.name;
      windSpeed.value = Math.round(data.wind.speed) + ' Km/h';
      humidity.value = data.main.humidity + '%';
      icon.value = data.weather[0].main;
      errorMessage.value = ''; // Clear any previous error message
    } else {
      // If the response is not OK, set error message based on response status
      errorMessage.value = response.status === 404 ? 'City not found. Please enter a valid city name.' : 'Error fetching weather data.';
    }
  } catch (error) {
    console.error('Error fetching weather data:', error);
  }
}

checkWeather();

</script>

<template>
  <div class="w_app">
    
    <div class="search-place" >
      <div class="input">
        <input 
        @submit.prevent="checkWeather()"
        v-model="location" 
        @keyup.enter="newItem" 
        placeholder="Type city name" 
        size="10"
        >
      </div> 
      <button @click="newItem">
        <img src="..//img/search_icon.png" alt="">
      </button>      
    </div>
        <div class="error" v-if="errorMessage !== ''">
      <h6>{{ errorMessage }}</h6>
    </div>
    <div class="weather">
        <div v-if="icon === 'Clouds'"><img src="..//img/clouds.png" alt=""></div>
        <div v-else-if="icon === 'Clear'"><img src="..//img/clear.png" alt=""></div>
        <div v-else-if="icon === 'Rain'"><img src="..//img/Rain.png" alt=""></div>

        <div class="temp">        
          <h1>{{ temp }}</h1>
          <h2><img src="..//img/place.png" alt=""> {{ cityName }}</h2>
        </div>
    </div>
      
      <div class="details">
        <div class="wind">
          <img src="..//img/wind.png" alt="">
          <h3>{{ windSpeed }}</h3>
        </div>
        <div class="humidity">
          <img src="..//img/humidity_percentage.png" alt="">
          <h4>{{ humidity }}</h4>
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
  color: #000000;
  margin: 0 auto;
  background-color: #000000;;
}


.w_app{
  padding: 15px;
  margin: 0 auto;
  width: 550px;
  height: auto;
  border-radius: 25px;
  background:linear-gradient(15deg, #13547a , #80d0c7 );

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
  color: #303030;
  font-size: 18px;
  border-radius: 25px;
  display: flex;
  align-items: center;
  }

  .input input{
    width:90%;
    border: 0;
    outline: 0;
    background: #ffffff;
    color: #303030;
    height: 40px;
    margin-left: 15px;
    font-size: 18px;
  }

.search-place button {
  width: 50px;
  height: 50px;
  margin-left: 10px;
  border-radius: 25px;
  border: 0;
  outline: 0;
  background: #ffffff;
  color: #303030;
}

.search-place button img {
  width: 25px;

}

.error {
  width: 100%;
  height:50px;
  border-radius: 5px;
  border: 0;
  outline: 0;
  background: #ffe2e2;
  color: #ff0000;
  display: flex;
  font-size: 28px;
  justify-self: center;
  align-items: center;
}

.weather  {
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
.temp img {
  width: 30px;
  height: auto;
}
.temp h1 {
  font-size: 80px;
}
.temp h2 {
  font-size: 40px;
  top: 20px;
}

.details {
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
  font-size: 28px;
}

.wind img, .humidity img {
  width: 50px;
  margin: 10px 0 10px 0;
}

@media (max-width: 540px) {
  /*  
  body {
    display: flex;
    place-items: center;
  }
  #app {
    display: flex;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }
  */

  .w_app{
    width: 100%;
    height: 100vh;
    border-radius: 0;
  }
  
.input {
  width:90%;

  }

  .input input{
    width:90%;
    border: 0;
    outline: 0;
    background: #ffffff;
    color: #303030;
    height: 40px;
    margin-left: 15px;
    font-size: 18px;
  }

  .search-place button {
  width: 50px;
  height: 50px;
  border: 0;
  outline: 0;
  background: #ffffff;
  color: #303030;
  }
}
</style>
