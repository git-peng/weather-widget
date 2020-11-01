<template>
    <div class="button-position">
      <v-tooltip v-if="!$vuetify.theme.dark" bottom>
        <template v-slot:activator="{ on }">
          <v-btn v-on="on" color="info" small fab @click="toggleMode">
            <v-icon class="mr-1">mdi-moon-waxing-crescent</v-icon>
          </v-btn>
        </template>
        <span>Dark Mode On</span>
      </v-tooltip>

      <v-tooltip v-else bottom>
        <template v-slot:activator="{ on }">
          <v-btn v-on="on" color="info" small fab @click="toggleMode">
            <v-icon color="yellow">mdi-white-balance-sunny</v-icon>
          </v-btn>
        </template>
        <span>Dark Mode Off</span>
      </v-tooltip>
    </div>
</template>

<script>
    export default {
      name:'DarkToggle',

      methods: {
          toggleMode() {
              this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
          }
      }
    }
</script>

<style scoped>
  @media (max-width: 500px) {
       .button-position {
        position: relative;
        right: 15px;
    }
  }
  @media (min-width: 501px) {
     .button-position {
      position: relative;
      top: -10px;
      right: -10px;
    }
  }
</style>