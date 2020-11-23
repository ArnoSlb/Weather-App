<template>
    <div class="weather"> 
        <div class="container">
            <h1>Hello Weather</h1>
            <div>
            <label for="position">Search City</label>
            <input 
                id="position" 
                type="text"
                class=""
                v-model="requete"
                v-on:keypress="goWeather"
            >
                <div>
                    <h3>Position: Paris</h3>
                    <div>
                        <p>Temperature : 20°C</p>
                        <p>Temps: Nuageux</p>
                        
                    </div>
                </div>
            </div>
        </div> 
        <div class="title-infos">
            <div class="temperature">
                <h1 class="title-temperature">16</h1>
                <p class="temperature-logo">°</p>
            </div>
            <div class="date-city-icon">
                <div class="date-time-infos">
                    <h2 class="current-city">London</h2>
                    <div>
                        <p class="current-date">{{currentDate}}</p>
                    </div>
                </div>
                <div class="current-weather">
                    <img class="icon-current-weather" src="../assets/weather-pack-icon/001-storm-12.png" alt="">
                    <p class="text-current-weather">Storm</p>
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
    var arr_month=new Array("January", "February", "March", "April", "May", "June", "July","August","September","October","November","December");
	var day = today.getDate(); // retourne le jour (1à 31)
	var dayOfTheWeek = today.getDay() ; // retourne un entier compris entre 0 et 6 (0 pour dimanche)
	var arr_day=new Array("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday");

const currentDate = arr_day[dayOfTheWeek] + ', ' + day + ' ' + arr_month[month] + ' ' + year ; 
// console.log(currentDate, today);
        

export default {
    name: 'Weather',
    data(){
        return {
            currentDate: currentDate,
            requete: '',
            weather: undefined,
            api_code: '625ff0820b405788a4e383dacc79095a',
            url_recherche: 'https://api.openweathermap.org/data/2.5/weather?'
        }
    },
    methods: {
        goWeather(event){
            if(event.key == "Enter"){
                axios.get(`${this.url_recherche}q=${this.requete}&units=metric&APPID=${this.api_code}`)
                .then(reponse => {
                    console.log(reponse);
                    this.weather=reponse.data
                })
                .finally(
                    this.requete=''
                )
            }
        }
    }
}

</script>

<style>
    @import './style.css'
</style>