<template>
  <!-- Don't drop "q-app" class -->
  <div id="app">
    <header></header>
    <main>
        <div style="float:left;margin-left:40px">
        <h4>{{location.localtime.substr(11,5)}}</h4>
          <h5>{{location.name}}</h5>
          <h6>{{location.country}}</h6>
        </div>
         <div style="float:right;margin-right:40px">
          
          <table>
          <tr>
          <td><img style="float:left" :src="'http:'+currentWeather.condition.icon" alt="">
         </td>
          <td> <p style="font-size:24px;margin-top:10px">{{currentWeather.temp_c}} <sup>o</sup></p></td>
          </tr>
          <tr>
          <td colspan="2">
          <p>{{currentWeather.condition.text}}</p>
          </td>
          </tr>
          <tr>
          <td colspan="2">
          {{forecastWeather.forecastday[0].date}}
          </td>
          </tr>
          <tr v-bind:key="hour" v-for="hour in forecastWeather.forecastday[0].hour">
          <td>{{hour.temp_c}} <sup>o</sup></td>
          <td><img :src="'http:'+hour.condition.icon" alt=""></td>
          <td>{{hour.time.substr(11,5)}}</td>

          </tr>
          </table>

         </div>

      
      
      <router-view></router-view>
    </main>
  </div>
</template>

<script>
/*
 * Root component
 */
import axios from 'axios'
export default {
  name:'app',
  data(){
    return {
        currentWeather:[],
        forecastWeather:[],
        location:[]
    }
  },
  methods:{
    getWeatherInfo(){
      axios.get("https://api.apixu.com/v1/forecast.json?key=//YOURKEYHERE&q=Istanbul").then(response=>{
        this.currentWeather=response.data.current;
        this.forecastWeather=response.data.forecast;
        this.location=response.data.location;
        console.log(response.data);
      })
    }
  },
  mounted(){
    this.getWeatherInfo();
  }
}
</script>

<style lang="stylus">
@import '~variables'

main
  text-align center
  margin-top 40px

header
  margin 0
  height 10px
  background-color $primary
</style>
