<template>
  <v-card class="mx-auto no-box-shadow">
    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="headline">
          <v-row>
            <v-col cols="10">{{location}}</v-col>
            <v-col cols="2"><DarkToggle/></v-col>
          </v-row>
        </v-list-item-title>
        <v-list-item-subtitle>{{dailyData.date}}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text class="weather-info">
      <v-row align="center">
        <v-col cols="6" class="remove-top-padding">
          <v-img :src="imgSrc" :alt="imgAlt" width="154"></v-img>
        </v-col>
        <v-col class="display-1" cols="6">
          {{dailyData.weather[0].description}}
        </v-col>
      </v-row>
    </v-card-text>

    <v-card-text class="weather-info">
      <v-row align="center">
        <v-col class="display-1" cols="6" >
          <v-icon class="rotate-90">mdi-send</v-icon>{{minTemp}}&deg;C
        </v-col>
        <v-col class="display-1" cols="6">
          <v-icon class="rotate-270 ">mdi-send</v-icon>{{maxTemp}}&deg;C
        </v-col>
      </v-row>
    </v-card-text>

    <v-divider></v-divider>
    <br>

    <v-list class="weather-info-list">
      <v-list-item class="list-item">
        <v-list-item-title>Wind</v-list-item-title>
        <v-list-item-subtitle>{{dailyData.wind_speed}} M/S</v-list-item-subtitle>
      </v-list-item>
      <!-- current weather doesn't have uvi, so replace with the other information -->
      <v-list-item class="list-item" v-if="uvi">
        <v-list-item-title>UVI</v-list-item-title>
        <v-list-item-subtitle>{{dailyData.uvi}}</v-list-item-subtitle>
      </v-list-item>
      <v-list-item class="list-item" v-else>
        <v-list-item-title>Temprature</v-list-item-title>
        <v-list-item-subtitle>{{temp}}&deg;C</v-list-item-subtitle>
      </v-list-item>

      <v-list-item class="list-item">
        <v-list-item-title>Humidity</v-list-item-title>
        <v-list-item-subtitle>{{dailyData.humidity}} %</v-list-item-subtitle>
      </v-list-item>
      <v-list-item class="list-item" v-if="dewpoint">
        <v-list-item-title>Dewpoint</v-list-item-title>
        <v-list-item-subtitle>{{dailyData.dew_point}}</v-list-item-subtitle>
      </v-list-item>
    </v-list>
  </v-card>
</template>

<script>
  import DarkToggle from './DarkToggle';
  export default {
    name: 'WeatherCard',

    props: ['dailyData','id', 'location'],
    components: {
      DarkToggle
    },
    data () {
      return {
        imgSrc: '', //the weather icon
        imgAlt: '', //text for the weather icon 
        minTemp: '',//min temprature 
        maxTemp: '',//max temprature
        temp: '', //current temprature
        uvi: '', 
        dewpoint: ''
      }
    },
    methods: {
      setupData() {
        this.imgSrc = "http://openweathermap.org/img/wn/"+this.dailyData.weather[0].icon+"@2x.png"
        this.imgAlt = this.dailyData.weather[0].description
        this.minTemp = Math.round(this.dailyData.temp.min)
        this.maxTemp = Math.round(this.dailyData.temp.max)
        this.temp = Math.round(this.dailyData.current_temp)
        this.uvi = this.dailyData.uvi
        this.dewpoint = this.dailyData.dew_point 
      }
    },

    mounted() {
      this.setupData()
    },
    
    watch: {
      dailyData: {
        handler() {
          this.setupData();
        }
      }
    }
  }
</script>

<style scoped>
  .weather-info {
    padding-top: 0;
    padding-bottom: 0;
  }

  @media (max-width: 500px) {
    .weather-info, .weather-info-list {
      margin-left: 10px;
    }
  }
  @media (min-width: 501px) {
    .weather-info, .weather-info-list {
          margin-left: 30px;
      }
  }

  .remove-top-padding{
    padding-top:0;
  }

  .list-item {
    min-height:32px;
  }
  .no-box-shadow {
    box-shadow: none !important;
    margin-top: 10px !important;
  }

  /*rotate the horizontal arrow to vertical*/
  .rotate-90 {
    -webkit-transform: rotate(90deg);
    -moz-transform: rotate(90deg);
    -ms-transform: rotate(90deg);
    -o-transform: rotate(90deg);
    transform: rotate(90deg);
  }

  .rotate-270 {
    -webkit-transform: rotate(270deg);
    -moz-transform: rotate(270deg);
    -ms-transform: rotate(9270deg);
    -o-transform: rotate(270deg);
    transform: rotate(270deg);
  }

</style>

