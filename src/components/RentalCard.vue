<template>
  <v-card class="mx-auto" max-width="344">
    <v-img :src="imageUrl" height="200px"></v-img>
    <v-card-title> {{ rentalDetails.name }}</v-card-title>
    <v-card-subtitle> {{ location }} </v-card-subtitle>
    <v-card-subtitle> {{ price }} </v-card-subtitle>
    <v-card-actions>
      <v-btn color="orange lighten-2" text>
        Book now
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn icon @click="show = !show">
        <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
      </v-btn>
    </v-card-actions>
    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
        <v-card-text>
          {{ descriptionsFirstFourSentances }}
        </v-card-text>
        <ul>
          <li>sleeps: {{ rentalDetails.numPeople }}</li>
          <li>rooms: {{ rentalDetails.numBedroom }}</li>
          <li>bathrooms: {{ rentalDetails.numBathroom }}</li>
        </ul>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script>
export default {
  name: 'RentalCard',
  props: {
    rentalDetails: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  data() {
    return {
      show: false
    };
  },
  computed: {
    descriptionsFirstFourSentances() {
      return this.rentalDetails.descriptions
        .split('.')
        .slice(0, 4)
        .join('.');
    },
    imageUrl() {
      const { id } = this.rentalDetails;
      return `https://picsum.photos/id/${id}/200/300`;
    },
    price() {
      const price = this.rentalDetails['prices.price'].split(' ')[1];
      const duration = this.rentalDetails['prices.period'];
      return `$${price} / ${duration}`;
    },
    location() {
      return `${this.rentalDetails.city}, ${this.rentalDetails.country}`;
    }
  }
};
</script>
