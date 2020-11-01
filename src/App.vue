<template>
  <v-app>
    <v-main>
      <v-container>
        <div class="mx-auto theme--light"> 
          <v-row align="center" class="search-row">
              <v-text-field v-model="search" label="Location?" v-on:keydown="enterKeyPress"></v-text-field>
              <v-btn icon @click="searchButtonClick">Search</v-btn>
          </v-row>
          <Carousel :weatherData="weatherData" :location="location" :key="location" :hideDelimiter="hideDelimiter"/>
          <BottomActionBar @click-event="handleChildCall" @current-event="getCurrent"/>
        </div>
        <!-- This dialog only for display error or alert messages -->
        <div class="text-center">
          <v-dialog v-model="dialog" width="300" persistent>
            <v-card>
              <v-card-text>
                <br>
                <h2>Alert</h2>
                <br>
                {{errorMessage}}
              </v-card-text>
              <v-divider></v-divider>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn  color="primary" text @click="dialog = false, errorMessage=''"> Ok</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';
import dateformat from 'dateformat'; //https://www.npmjs.com/package/dateformat
import Carousel from './components/Carousel';
import BottomActionBar from './components/BottomActionBar';

export default {
  name: 'App',

  components: {
    Carousel,
    BottomActionBar,
  },

  data() {
    return {
      search: '', //location from input
      weatherData: [], //the data from open weather
      location: '', // location that return from google api call or default value
      latitude: '', // latitude from google or default value
      longitude: '', // longitude from google or default value
      hideDelimiter: false, //hide the carousel delimiter when check current weather information
      errorMessage: '', // error or alert message
      dialog: false, // show / hide error message dialog
    }
  },

  methods: {
    getCoordinates() {
      return new Promise(function(resolve, reject) {
        navigator.geolocation.getCurrentPosition(resolve, reject);
      });
    },

    async getLocation() {
      if(this.search === '') {
        try {
          let res = await axios.get("https://maps.googleapis.com/maps/api/geocode/json?latlng="
                                    +this.latitude+","+this.longitude+"&key="+process.env.VUE_APP_GOOGLE_API_KEY+"&result_type=administrative_area_level_2")
          if(res.status == 200) {
            console.log("google coordinates search is ok")
          }
          return res.data
        }catch(err) {
          this.setErrorMessage(err)
        }
      }else {
         try {
           let res = await axios.get("https://maps.googleapis.com/maps/api/geocode/json?address="+this.search+"&key="+process.env.VUE_APP_GOOGLE_API_KEY)
           if(res.status == 200) {
            console.log("google location search is ok")
              console.log(res.data)
            }
           return res.data
         }catch(err) {
          this.setErrorMessage(err)
        }
      }
    },

    async getWeatherData() {
      try {
        let res = await axios.get("https://api.openweathermap.org/data/2.5/onecall?lat="
                                  +this.latitude+"&lon="+this.longitude+"&units=metric&appid="+process.env.VUE_APP_OPEN_WEATHER_API_KEY)
        if(res.status == 200) {
            console.log("weather search is ok")
          }
          return res.data
      }catch(err) {
          console.err(err)
          this.setErrorMessage(err)
      }
    },

    async getCurrentWeatherData() {
      try {
        let res = await axios.get("https://api.openweathermap.org/data/2.5/weather?lat="+this.latitude+"&lon="+this.longitude+"&units=metric&appid="
                                  +process.env.VUE_APP_OPEN_WEATHER_API_KEY)
        if(res.status == 200) {
            console.log("current weather search is ok")
          }
          console.log(res.data)
          return res.data
      }catch(err) {
        console.err(err)
        this.setErrorMessage(err)
      }
    },

    setLatAndLng(lat, lng) {
      this.latitude=lat
      this.longitude=lng
    },

    async getCurrent() {
      if(this.latitude === '' && this.longitude === '') {
          var position = await this.getCoordinates()
          this.setLatAndLng(position.coords.latitude, position.coords.longitude)
          this.getLocation().then(res => {
            this.location=res.results[0].formatted_address
            this.setLatAndLng(res.results[0].geometry.location.lat, res.results[0].geometry.location.lng)
            this.getCurrentWeatherData().then(res =>{
              this.setCurrentWeatherData(res);
            })
          }).catch(()=>{
            this.setErrorMessage("Can't find the location")
          })
      }else {
        this.getCurrentWeatherData().then(res =>{
              this.setCurrentWeatherData(res);
            }).catch(()=>{
              this.setErrorMessage("Can't get the Weather data")
          })
      }
      this.hideDelimiter = true
    },

    setCurrentWeatherData(res) {
      this.weatherData=[];
      this.weatherData=[{
        "date": dateformat(new Date(),"dddd, mmmm dS, yyyy, h:MM:ss TT"),
        "weather": res.weather,
        "temp": {"min":res.main.temp_min, "max":res.main.temp_max},
        "wind_speed": res.wind.speed,
        "uvi": '',
        "humidity": res.main.humidity,
        "dewpoint":'',
        //current information
        "current_temp": res.main.temp
      }];
      console.log(this.weatherData);
    },

    async getAllData() {
      if (this.search === '') {
        var position = await this.getCoordinates()
        this.setLatAndLng(position.coords.latitude, position.coords.longitude)
      } 
      this.getLocation().then(res => {
        this.location=res.results[0].formatted_address
        //yes, the values could be set in above, but a check is really not neccessary  
        this.setLatAndLng(res.results[0].geometry.location.lat, res.results[0].geometry.location.lng)

        this.getWeatherData().then(res => {
          this.weatherData=[]
          const today = new Date()
          //display 7 days data
          let days = res.daily.length>7 ? 7 : res.daily.length
          for(var i=0; i<days; i++) {
              let finalDate = new Date(today)
              finalDate.setDate(today.getDate()+i)
              if(i===0) {
                  //add time for today
                  res.daily[i].date=dateformat(finalDate, "dddd, mmmm dS, yyyy, h:MM:ss TT") 
                }else {
                  //only date but no time display for the other days
                  res.daily[i].date=dateformat(finalDate, "dddd, mmmm dS, yyyy") 
              }
              this.weatherData[i]=res.daily[i]
          }
              console.log(this.weatherData)
        }).catch(()=>{
              this.setErrorMessage("Can't get the Weather data")
          })
      }).catch(()=>{
        this.setErrorMessage("Can't find the location")
      })
      //clear search term
      this.search=''
      this.hideDelimiter = false
    },

    searchButtonClick() {
      if(this.search === '') {
        this.setErrorMessage("Please type a location to search.")
        return
      }
      this.getAllData()
    },

    enterKeyPress(event) {
      if(event.keyCode === 13) {
        this.searchButtonClick()
      }    
    },

    handleChildCall() {
      this.getAllData()
    },

    setErrorMessage(msg) {
      this.dialog=true
      this.errorMessage=msg
    }

  },

  mounted() {
    this.getAllData()
  }
};
</script>

<style scoped>
  @media (max-width: 500px) {
      .container {
        max-width: 100% !important; 
        min-width: 390px;
      }
  }

  @media (min-width: 501px) {
      .container {
        width: 500px !important;
      }
  }
  .search-row {
    margin-left: 5px;
    margin-right: 20px;
  }
</style>