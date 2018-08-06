<template>



  <v-container fluid>
       <v-text-field
            label="EnterCity"
            v-model="address"
          ></v-text-field>


      <v-list>{{getDate()}}</v-list>
       <v-list>{{place}}</v-list>

      <v-list> <v-layout><v-flex><img :src="icon">{{temperature}}</v-flex><v-flex>{{description}}</v-flex></v-layout></v-list>



    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>

        <blockquote>

          <footer>
            <small>
              <em>{{info}}</em>
            </small>
          </footer>
        </blockquote>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
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
</style>
<script>
import axios from 'axios'

export default {
  data () {
    return {
      getDate () {
        const toTwoDigits = num => num < 10 ? '0' + num : num;
        let today = new Date()
        let year = today.getFullYear()
        let month = toTwoDigits(today.getMonth() + 1)
        let day = toTwoDigits(today.getDate())
        return `${year}-${month}-${day}`
      },
      info: null,
      temperature: '',
      currentLocation: { lat: 0, lng: 0},
      searchAddressInput: '',
      async: true,
      place: '',
      country: '',
      icon: '',
      description: '',
      address: ''
    }
  },
  created () {
    axios.get('https://fcc-weather-api.glitch.me/api/current?lat=27.688495000000003&lon=85.33945489999999')
      .then((response) => {
        this.info = response.data
        this.place = this.info.name
        this.icon = this.info.weather[0].icon
        this.description = this.info.weather[0].description
        this.temperature = this.info.main.temp
      })
    axios.get('https://maps.googleapis.com/maps/api/js?sensor=false')
        .then((callback, address) => {
          address = address || 'kathamadu'
          geocoder = new google.maps.Geocoder();
        })
  },
  mounted: function () {
    this.geolocation()
  },

  methods: {
    geolocation: function () {
      navigator.geolocation.getCurrentPosition((position) => {
        this.currentLocation = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        }
      })
    },
    getLatitudeLongitude: function (callback, address) {
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

  }
}
</script>
