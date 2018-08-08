<template>



  <v-container fluid class="wraper">
       <v-text-field
            label="EnterCity"
            v-model="address"
          ></v-text-field>
    {{address}}

<v-container>
  <h1>{{place}}</h1>
  <h2>{{getDate()}}</h2>
  <h2>{{description}}</h2>
  <v-layout>
  <v-flex><img :src="icon"><h1>{{temperature}} C</h1></v-flex>
  </v-layout>
  </v-container>



    <v-slide-y-transition mode="out-in">


    </v-slide-y-transition>
    <v-flex xs12 sm6 class="py-2">

      <v-btn-toggle>
        <v-btn flat value="left">
          Temperature
        </v-btn>
        <v-btn flat value="center">
          Percipitation
        </v-btn>
        <v-btn flat value="right">
          Wind
        </v-btn>


      </v-btn-toggle>
    </v-flex>
  </v-container>
</template>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
 v-list{
   margin:0px;
   padding:0px;
 }
h1, h2 {
  padding: 0px;
  margin:0px;
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
  .wraper{
    width:500px;
    height:500px;
    border: black 2px solid;
  }
</style>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      getDate () {
        let today = new Date()
        let minutes = today.getMinutes()
        let hour = today.getHours()
        let weekday = ['sunday', 'monday', 'Tuesday', 'wednesday', 'Thrusday', 'Friday', 'Saturday']
        let day = weekday[today.getDay()]
        return `${day} ${hour} : ${minutes}`
      },
      info: null,
      temperature: '',
      currentLocation: {lat: 0, lng: 0},
      searchAddressInput: '',
      async: true,
      place: '',
      country: '',
      icon: '',
      description: '',
      address: ''
    }
  },
  mounted () {
    let goglescript = document.createElement('script')
    goglescript.setAttribute('src', 'https://maps.googleapis.com/maps/api/js?v=3.exp&sensor=false')
    document.head.appendChild(goglescript)
    function getlatlon (callback, address) {
      address = address || 'Kathamandu'
      geocoder = new google.maps.Geocoder()
      if (geocoder) {
        geocoder.geocode({
          'address': address
        }, function (results, status) {
          if (status == google.maps.GeocoderStatus.OK) {
            callback(results[0]);
          }
        })
      }
    }
  },
  created () {
    axios.get('https://fcc-weather-api.glitch.me/api/current?lat=27.7172453&lon=85.3239605')
      .then((response) => {
        this.info = response.data
        this.place = this.info.name
        this.icon = this.info.weather[0].icon
        this.description = this.info.weather[0].description
        this.temperature = this.info.main.temp
        this.pressure = this.info.
      })
  }
}
</script>
