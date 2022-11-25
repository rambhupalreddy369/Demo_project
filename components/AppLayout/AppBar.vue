<template>
  <header>
    <v-toolbar color="#5D4037" height="60">
      <v-icon dark left @click="backToHome"> mdi-arrow-left </v-icon>
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>Demo Card</v-toolbar-title>

      <v-spacer></v-spacer>
    </v-toolbar>
  </header>
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
