<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div id="container">
            <div class="weatherBox" :class="(typeof weatherDatas.main != 'undefined' && weatherDatas.main.temp > 15 ? 'warm' : '')">

                <div class="searchBox">
                    <input id="inputSearch" type="text" v-model="query" class="searchBar" placeholder="Recherche....." @keyup="callData">
                </div>
    
                <div class="cityData" v-if="(typeof weatherDatas.main != 'undefined')">
                    <div class="cityBox">
                        <div class="cityName" v-if="weatherDatas.name && weatherDatas.sys">{{weatherDatas.name}},    {{ weatherDatas.sys.country }}</div>
                        <div class="date" v-if="showDate"> {{ showDate() }}</div>
                    </div> 
                </div>
                
                <div class="weatherData">
                    <div class="weatherDeg" v-if="weatherDatas.main"><span>{{ Math.round(weatherDatas.main.temp) }}°c</span></div>
                    <div class="temp" v-if="weatherDatas.weather">{{ weatherDatas.weather[0].description }}</div>
                </div>
                
            </div>
    </div>
</template>


<script>
import axios from 'axios'

export default {
  data() {
    return {
      url_base : 'https://api.openweathermap.org/data/2.5/',
      api_key : '39f7c130f728cb36c9269b63c71d8f85',
      query : "",
      weatherDatas : {

      },
      imgHot : '../assets/image1.png',
      imgCold : '../assets/image2.jpg'
    }
  },
    methods: {
        callData(e) {
            if (e.key == "Enter") {
                axios.get(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}&lang=fr&units=metric&`)
                .then(response => {
                    // let results = response.data
                    this.weatherDatas =  response.data
                    // this.weather = results
                    console.log(response.data)
                }) 
            }
        } ,

        showDate() {
            let d = new Date()
            let months = ["Janvier", "Fevrier", "Mars", "Avril", "Mai", "Juin", "Juillet", "Aout", "Septembre", "Octobre", "novembre", "Decembre"]
            let days = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"]

            let day = days[d.getDay()]
            let date = d.getDate()
            let month = months[d.getMonth()]
            let years = d.getFullYear();

            return `${day} ${date} ${month} ${years}`
        }

    }
}
</script>

<style scoped>
    body{
        margin: 0px;
        padding: 0px;
    }
    #container{
        margin: 0px;
        padding: 0px;
        width: 100%;
    }
    .weatherBox{
        width: 50%;
        height: 100vh;
        margin: 0 auto;
        padding: 10px;
        background-image: url(../assets/image2.jpg);
        background-repeat: no-repeat;
        background-size: cover;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .weatherBox.warm{
        background-image: url(../assets/image1.png);
        background-repeat: no-repeat;
        background-size: cover;
    }
    #inputSearch{
        width: 450px;
        margin: 15px auto;
        height: 50px;
        text-align: center;
        font-size: 1em;
        font-family: 'Lato', sans-serif;
        font-weight: bold;
        border-radius: 0px 15px 0px 15px;
        border: none;
    }
    .cityName{
        font-family: 'Lato', sans-serif;
        font-weight: bold;
        font-size: 2em;
        margin: 20px 0px;
    }

    .weatherData{
       
    }
    .weatherDeg{
        padding: 10px;
        width: 200px;
        height: 130px;
        /* background-color: white; */
        display: flex;
        font-family: 'Lato', sans-serif;
        justify-content: center;
        align-items: center;
        border-radius: 20px;
        margin: 20px 0px;
        -webkit-box-shadow: 13px 9px 15px 5px rgba(0,0,0,0.33); 
            box-shadow: 13px 9px 15px 5px rgba(0,0,0,0.33);
    }
    .weatherDeg span{
        color: white;
        font-weight:700;
        font-size: 5em;
    }
    .temp{
        text-align: center;
        margin: 20px 0px;
        color: white;
        font-weight:700;
        font-size: 3em;
    }

</style>