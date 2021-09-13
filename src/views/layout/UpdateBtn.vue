<template>
    <v-tooltip left>
     <template v-slot:activator="{ on, attrs }">
    <v-btn
      class="mx-2 cloud"
      fab
      dark
      large
      bottom
      right
      color="teal"
      elevation="2"
      v-bind="attrs"
      v-on="on"
    >
      <v-icon dark>
        mdi-cloud-upload
      </v-icon>
    </v-btn>
    </template>
    <span>Update From Server</span>
    </v-tooltip>
</template>
<style scoped>
.cloud {
  position: fixed !important;
  bottom: 30px !important;
  right: 20px !important;
}
.bt-wrap {
  position: relative !important;
}
</style>