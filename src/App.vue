<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <h1>Live Reviews</h1>
      </div>

      <v-spacer></v-spacer>
      <v-btn v-if="!($route.name === 'New')" @click="redirectNew">+</v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
  methods:{
    redirectNew(){
      this.$router.push("/new");
    }
  }
};
</script>
