<template>
  <v-card elevation="24" max-height="600" class="mx-auto">
    <v-carousel
      :dark="$vuetify.theme.dark" 
      :light="!$vuetify.theme.dark"
      :continuous="false"
      :show-arrows="false"
      hide-delimiter-background
      delimiter-icon="mdi-minus"
      height="540"
      :hide-delimiters="hideDelimiter"
    >
      <v-carousel-item v-for="(daily, i) in weatherData" :key="i">
        <v-sheet height="100%">
          <WeatherCard :dailyData="daily" :location="location" />
        </v-sheet>
      </v-carousel-item>
    </v-carousel>
  </v-card>
</template>

<script>
  import WeatherCard from './WeatherCard';
  export default {
    name: 'Carousel',

    props: ['location', 'weatherData', 'hideDelimiter'],
    components: {
      WeatherCard
    }
  }
</script>