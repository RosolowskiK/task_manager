<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div id="logo">
        Task Manager
      </div>

      <div id="navigation">
        <router-link to="/">Home</router-link>
      </div>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  //
  // components: {
  //
  // },

  data: () => ({
    //
  })
}
</script>

<style>
#navigation a {
  color: black !important;
}
#logo {
  margin-right: 10px;
}
</style>
