
<template>
    <div>
        <div class="loader-background" v-if="loading">
            <div class="loader">
                <h1 class="loader-title">Chargement</h1>
                <div class="inner one"></div>
                <div class="inner two"></div>
                <div class="inner three"></div>
            </div>
        </div>
        <div v-else class="weather"> 
            <div class="container">
                <h1 class="title-app">Station Météo</h1>
                <div>
                <label class="position-text-search" for="position">Chercher une ville </label>
                <input 
                    id="position" 
                    type="text"
                    class="searching-city"
                    v-model="requete"
                    v-on:keypress="handler"
                >
                    <div>
                        <h3>Détails de la météo pour {{weather.name}}</h3>
                        <div class="data-scroll">
                            <h4 class="weather-description">{{weather.weather[0].description}}</h4>
                        <h4 class="categories">Les températures</h4>
                        <div class="details-temp-box">
                            <div class="details-infos-box" id="temp-min">
                                <img class="thermometer-icon" src="../assets/weather-pack-icon/024-thermometer-5.png" alt="">
                                <p class="details-text">min</p>
                                <p class="thermometer-number">{{weather.main.temp_min.toFixed()}} °C</p>
                            </div>
                            <div class="details-infos-box" id="temp">
                                <img class="thermometer-icon" src="../assets/weather-pack-icon/024-thermometer-5.png" alt="">
                                <p class="thermometer-number">{{weather.main.temp.toFixed()}} °C</p>
                            </div> 
                            <div class="details-infos-box" id="temp-feel">
                                <img class="thermometer-icon" src="../assets/weather-pack-icon/024-thermometer-5.png" alt="">
                                <p class="details-text">ressentis</p>
                                <p class="thermometer-number">{{weather.main.feels_like.toFixed()}} °C</p>
                            </div>  
                            <div class="details-infos-box" id="temp-max">
                                <img class="thermometer-icon" src="../assets/weather-pack-icon/024-thermometer-5.png" alt="">
                                <p class="details-text">max</p>
                                <p class="thermometer-number">{{weather.main.temp_max.toFixed()}} °C</p>
                            </div>   
                        </div>
                        <h4 class="categories">Anémométrie</h4>
                        <div class="details-anenometrie-box">  
                            <div class="details-infos-box">
                                <img class="humidity-icon" src="../assets/weather-pack-icon/humidity.png" alt="">
                                <p class="details-text">humidité</p>
                                <p class="thermometer-number">{{weather.main.humidity.toFixed()}} %</p>
                            </div>   
                            <div class="details-infos-box">
                                <img class="thermometer-icon" src="../assets/weather-pack-icon/027-wind-sign.png" alt="">
                                <p class="details-text">vitesse</p>
                                <p class="thermometer-number">{{weather.wind.speed}} km/h</p>
                            </div>   
                            <div class="details-infos-box">
                                <img class="thermometer-icon" src="../assets/weather-pack-icon/barometer.png" alt="">
                                <p class="details-text">pression</p>
                                <p class="thermometer-number">{{weather.main.pressure.toFixed()}} hPa</p>
                            </div> 
                        </div>
                        <h4 class="categories">Aurore et Crépuscule</h4>
                        <div class="details-sunrise-box">  
                            <div class="details-sunrise-infos-box">
                                <img class="humidity-icon" src="../assets/weather-pack-icon/sunrise.png" alt="">
                                <p class="details-text">levée (heure française)</p>
                                <p class="thermometer-number">{{sunrise}}</p>
                            </div>   
                            <div class="details-sunrise-infos-box">
                                <img class="thermometer-icon" src="../assets/weather-pack-icon/sunset.png" alt="">
                                <p class="details-text">couchée (heure française)</p>
                                <p class="thermometer-number">{{sunset}}</p>
                            </div>   
                        </div>
                        <h4 class="categories">La météo des prochaines 24h par intervalles de 3h</h4>
                        <table class="table-week">
                            <tr class="table-categories">
                                <td class="table-categories-categorie">Heure</td>
                                <td class="table-categories-categorie">Météo</td>
                                <td class="table-categories-categorie">Température</td>
                                <td class="table-categories-categorie">Humidité</td>
                                <td class="table-categories-categorie">Vent</td>      
                            </tr>   
                            <tr v-for="hour in weatherByDay" :key="hour.dt_txt" class="table-data">
                                <td class="table-data">{{hour.dt_txt}}</td>
                                <td class="table-data">{{hour.weather[0].description}}</td>
                                <td class="table-data">{{hour.main.temp.toFixed() - 273}} °C</td>
                                <td class="table-data">{{hour.main.humidity}} %</td>
                                <td class="table-data">{{hour.wind.speed}} km/h</td>      
                            </tr>  
                        </table>
                        </div>     
                    </div>
                </div>
            </div> 
            <div class="title-infos">
                <div class="temperature">
                    <h1 class="title-temperature">{{weather.main.temp.toFixed()}}</h1>
                    <p class="temperature-logo">°</p>
                </div>
                <div class="date-city-icon">
                    <div class="date-time-infos">
                        <h2 class="current-city">{{weather.name}}</h2>
                        <div>
                            <p class="current-date">{{currentDate}}</p>
                        </div>
                    </div>
                    <!-- <div class="current-weather">
                        <img class="icon-current-weather" id="wicon" v-bind:src="iconUrl" alt="Weather icon">
                        <p class="text-current-weather">{{weather.weather[0].description}}</p>
                    </div> -->
                    
                </div>
            </div>
        </div>
    </div>
    
</template>

<script>

import axios from 'axios';


var today = new Date(); 
	var year = today.getFullYear(); // retourne le millésime
    var month = today.getMonth(); // date.getMonth retourne un entier entre 0 et 11 
    var arr_month=new Array("Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet","Aout","Septembre","Octobre","Novembre","Decembre");
	var day = today.getDate(); // retourne le jour (1à 31)
	var dayOfTheWeek = today.getDay() ; // retourne un entier compris entre 0 et 6 (0 pour dimanche)
	var arr_day=new Array("Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi");

const currentDate = arr_day[dayOfTheWeek] + ' ' + day + ' ' + arr_month[month] + ' ' + year ; 


// let sunriseConvertion = new Date(sunriseUnix*1000)
// let sunrise = sunriseConvertion.getHours() + 'h' + sunriseConvertion.getMinutes()
// console.log(sunrise)
// console.log(currentDate, today);

// let currentPosition = null;
// console.log(currentPosition);

// if (navigator.geolocation) {
//     console.log('géolocalisation dispo');
//     getLocation();
// }

// function getLocation() {
//     navigator.geolocation.getCurrentPosition((position) => {
//         // return the coordinates
//         currentPosition = position.coords;
//     }, (error) => {
//         // check if the user denied geolocation, or if there was any other problem
//         if (error.code == error.PERMISSION_DENIED) {
//             alert('Geolocation has been disabled on this page, please review your browser\'s parameters');
//         } else {
//             alert('Unable to find your position, try again later.');
//         }
//     }, {
//         timeout: 10000
//     });
// }



export default {
    name: 'Weather',
    data(){
        return {
            currentDate: currentDate,
            requete: '',
            weather: undefined,
            api_code: '625ff0820b405788a4e383dacc79095a',
            url_recherche: 'https://api.openweathermap.org/data/2.5/weather?',
            sunriseUnix: 0,
            sunrise: '',
            sunsetUnix: 0,
            sunset:'',
            loading: true,
            weatherByDay: '',
            
        }
    },
    beforeMount() {
        this.goWeatherNantes();
        this.goWeatherNantesDay();
    },
    methods: {
        goWeatherNantes() {
            axios.get(`${this.url_recherche}q=nantes&units=metric&APPID=${this.api_code}&lang=fr`)
            .then(reponse => {
                        // console.log(reponse);
                        this.weather=reponse.data;
                        this.iconCode=reponse.data.weather[0].main;
                        this.sunriseUnix= new Date(reponse.data.sys.sunrise*1000);
                        this.sunrise=this.sunriseUnix.getHours() + 'h' + this.sunriseUnix.getMinutes();
                        this.sunsetUnix= new Date(reponse.data.sys.sunset*1000);
                        this.sunset=this.sunsetUnix.getHours() + 'h' + this.sunsetUnix.getMinutes();
                    })
                    .finally(
                        this.requete='',
                        this.loading=false,
                    )
        },
        goWeatherNantesDay() {
            axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=nantes&appid=${this.api_code}&lang=fr&cnt=9`)
            .then(reponse => {
                console.log(reponse);
                this.weatherByDay=reponse.data.list;
            })
            .finally(

            )
        },
        goWeather(event){
            if(event.key == "Enter"){
                axios.get(`${this.url_recherche}q=${this.requete}&units=metric&APPID=${this.api_code}&lang=fr`)
                .then(reponse => {
                    // console.log(reponse);
                    this.weather=reponse.data;
                    this.sunriseUnix= new Date(reponse.data.sys.sunrise*1000);
                    this.sunrise=this.sunriseUnix.getHours() + 'h' + this.sunriseUnix.getMinutes();
                    this.sunsetUnix= new Date(reponse.data.sys.sunset*1000);
                    this.sunset=this.sunsetUnix.getHours() + 'h' + this.sunsetUnix.getMinutes();
                })
                .finally(
                    // this.requete=''
                )
            }
        },
        goWeatherDay(event) {
            if(event.key == "Enter"){
                axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${this.requete}&appid=${this.api_code}&lang=fr&cnt=9`)
                .then(reponse => {
                    console.log(reponse);
                    this.weatherByDay=reponse.data.list;
                })
                .finally(
                    this.requete=''
                )
            }
        },
        sunriseFct(unix){
            let sunriseConvertion = new Date(unix*1000);
            let sunrise = sunriseConvertion.getHours() + 'h' + sunriseConvertion.getMinutes();
            return(sunrise);
        },
        handler(event){
            this.goWeather(event);
            this.goWeatherDay(event);
        }

    }
}

</script>

<style>
    @import './style.css'
</style>