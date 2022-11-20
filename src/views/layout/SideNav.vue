<template>
    <v-navigation-drawer
        dark
        app
        permanent
        expand-on-hover
      >
        <v-list>
          <v-list-item class="px-2" link>
            <v-list-item-avatar>
              <v-img src="https://pbs.twimg.com/profile_images/864282616597405701/M-FEJMZ0_400x400.jpg"></v-img>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title class="text-h6">
                {{userInfo.fullName}}
              </v-list-item-title>
              <v-list-item-subtitle>ADMIN</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list>

        <v-divider></v-divider>

        <v-list
          nav
          dense
        >
          <v-list-item link to="/">
            <v-list-item-icon>
              <v-icon>mdi-creation</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item>
          <v-list-item link to="products">
            <v-list-item-icon>
              <v-icon>mdi-shopping-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Product Controller</v-list-item-title>
          </v-list-item>
          <v-list-item link to="categories">
            <v-list-item-icon>
              <v-icon>mdi-format-list-bulleted-square</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Category Controller</v-list-item-title>
          </v-list-item>
          <v-list-item link to="orders">
            <v-list-item-icon>
              <v-icon>mdi-cart-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Orders</v-list-item-title>
          </v-list-item>
          <v-list-item link to="users">
            <v-list-item-icon>
              <v-icon>mdi-account-check</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Users</v-list-item-title>
          </v-list-item>
          
          <v-list-item link to="settings">
            <v-list-item-icon>
              <v-icon>mdi-power-settings</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Settings</v-list-item-title>
          </v-list-item>
        
        </v-list>

        <!-- nav footer -->
        
        <v-footer class="pa-0" style="display: inline-block" fixed>
          <v-list nav dense>

          <v-list-item link to="support">
            <v-list-item-icon>
              <v-icon>mdi-face-agent</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Support</v-list-item-title>
          </v-list-item>
           <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-copyright</v-icon>
              </v-list-item-icon>
              <v-list-item-title>ABS World Xpress</v-list-item-title>
            </v-list-item>
        </v-list>
        </v-footer>
    </v-navigation-drawer>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      onlineStatus: "Offline",
      AUTH_API: 'http://194.233.68.154:8082/auth/',
      auth: "Bearer " + localStorage.getItem("token"),
      userInfo: '',
    }
  },
  methods: {
    setOnlineStatus() {
      if(window.navigator.onLine){
        this.onlineStatus="Online";
        return true;
      }
      else
      {
        this.onlineStatus="Offline";
        return false;
      }
    },
    getProfile(){
      axios({
            method: "get",
            url: this.AUTH_API+`user-info?credentials=%7B%7D&details=%7B%7D&principal=%7B%7D`,
            headers: {
                Authorization: this.auth
            }
        })
        .then(r => {
            this.userInfo = r.data;
            console.log(this.userInfo)
                })
        .catch(r => {
            console.log(r)
        });
    }
  },
  mounted() {
    this.setOnlineStatus()
    this.getProfile();
  }
}
</script>

<style lang="scss" scoped>
.v-list-item__title {
  font-size: 15px !important;
}
.v-breadcrumbs__item a{
  font-weight: bolder !important;
}
</style>
