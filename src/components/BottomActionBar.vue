<template>
  <v-bottom-navigation
    color="primary"
  >
  <v-tooltip top>
    <template v-slot:activator="{ on }">
     <v-btn v-on="on" @click="callGetCurrentFromParent">
     <span>Current</span>
     <v-icon>mdi-history</v-icon>
     </v-btn>
    </template>
      <span>Check current weather</span>
    </v-tooltip>

   <v-tooltip top>
      <template v-slot:activator="{ on }">
        <v-btn v-on="on" @click="callGetDataFromParent" >
       <span>Local</span>
      <v-icon>mdi-map-marker</v-icon>
    </v-btn>
    </template>
      <span>Check local 7 days weather</span>
    </v-tooltip>
  </v-bottom-navigation>
</template>

<script>
  export default {
    props:['click-event'],
    data () {
      return {
          value: 1
      }
    },
    methods: {
        callGetDataFromParent() {
            this.$emit('click-event')
        },

        callGetCurrentFromParent() {
            this.$emit('current-event')
        }
    }
  }
</script>

<style scoped>
    .v-item-group.v-bottom-navigation {
        justify-content: flex-end;
    }
</style>