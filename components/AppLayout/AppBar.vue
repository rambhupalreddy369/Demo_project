<template>
  <v-toolbar color="indigo" dark>
    <v-icon dark left @click="backToHome"> mdi-arrow-left </v-icon>
    <v-app-bar-nav-icon></v-app-bar-nav-icon>

    <v-toolbar-title>Demo Card</v-toolbar-title>

    <v-spacer></v-spacer>
  </v-toolbar>
</template>

<script>
export default {
  methods: {
    backToHome() {
      this.$router.push("/");
    },
  },
};
</script>
