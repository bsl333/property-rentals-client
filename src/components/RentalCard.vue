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
          <div class="desciption">
            <h4>Description</h4>
            <div class="mt-1">
              {{ descriptionSummary }}
            </div>
          </div>
          <div class="details mt-5">
            <h4>Details</h4>
            <ul class="list mt-1">
              <li>sleeps: {{ rentalDetails.numPeople }}</li>
              <li>rooms: {{ rentalDetails.numBedroom }}</li>
              <li>bathrooms: {{ rentalDetails.numBathroom }}</li>
            </ul>
          </div>
          <div class="fees mt-5" v-if="fees.length">
            <h4>Fees</h4>
            <ul class="list mt-1">
              <li v-for="(fee, i) in fees" :key="i">
                {{ fee.type }}: {{ fee.amount }}
              </li>
            </ul>
          </div>
        </v-card-text>
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
    descriptionSummary() {
      return this.rentalDetails.descriptions
        .split('.')
        .slice(0, 3)
        .join('.');
    },
    imageUrl() {
      const { id } = this.rentalDetails;
      return `https://picsum.photos/id/${id}/200/300`;
    },
    price() {
      const price = this.rentalDetails['prices.price'];
      const duration = this.rentalDetails['prices.period'];
      return `${price} / ${duration}`;
    },
    location() {
      return `${this.rentalDetails.city}, ${this.rentalDetails.country}`;
    },
    fees() {
      if (!this.rentalDetails.fees) {
        return [];
      }
      const fees = JSON.parse(this.rentalDetails.fees);
      return fees;
    }
  }
};
</script>

<style lang="scss" scoped>
.list {
  list-style-type: none;
  padding: 0;
}
</style>
