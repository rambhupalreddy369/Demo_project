<template>
  <v-app>
    <app-bar></app-bar>
    <nuxt />
  </v-app>
</template>

<script>
import AppBar from "../components/AppLayout/AppBar.vue";
export default {
  components: {
    AppBar,
  },
  name: "DefaultLayout",
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Welcome",
          to: "/",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Inspire",
          to: "/inspire",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Vuetify.js",
    };
  },
};
</script>
