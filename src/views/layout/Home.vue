<template>
  <v-app>
  <v-app-bar
      elevation="0"
      app
      height="73px"
      style="border-bottom:1px solid #e7e7e7 !important"
      color="white"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 mr-2"
          contain
          min-width="100"
          src="../../assets/hero-logo.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        color="primary"
        to="rx-prescription"
        text
      >
        <v-icon>mdi-prescription</v-icon>
        <span class="mr-2">New Prescription</span>
      </v-btn>
    </v-app-bar>
    <side-nav></side-nav>
    <v-main>
     <router-view/>
    </v-main>
    <update-btn></update-btn>
</v-app>
</template>

<script>
import SideNav from '../layout/SideNav.vue'
import UpdateBtn from '../layout/UpdateBtn.vue'
export default {
  name: 'App',
  components: {
    'side-nav': SideNav,
    'update-btn': UpdateBtn
  },
  data () {
    return {
    }
  }
}
</script>

