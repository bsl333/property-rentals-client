<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-toolbar-title>Find Your Dream Rental</v-toolbar-title>
      </div>
    </v-app-bar>

    <v-main class="mt-10">
      <RentalList />
    </v-main>
  </v-app>
</template>

<script>
import RentalList from './components/RentalList';

export default {
  name: 'App',
  components: {
    RentalList
  }
};
</script>
