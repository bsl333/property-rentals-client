<template>
  <div class="rental-list-container">
    <div class="header">
      <div class="text-center">
        <v-pagination
          v-model="page"
          :length="total"
          :total-visible="8"
        ></v-pagination>
      </div>
    </div>
    <v-container>
      <v-row v-for="(rentalRow, i) in rentalRows" :key="i">
        <v-col v-for="rental in rentalRow" :key="rental.id" cols="12" sm="4">
          <RentalCard class="pa-2" :rentalDetails="rental" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import api from '../utils/api';
import RentalCard from './RentalCard';

const PAGE_SIZE = 9;
export default {
  name: 'RentalList',
  components: {
    RentalCard
  },
  data() {
    return {
      page: 1,
      rentals: [],
      total: 0
    };
  },
  computed: {
    rentalRows() {
      const maxColLen = 3;
      return this.rentals.reduce(
        (acc, val) => {
          const lastRow = acc[acc.length - 1];
          lastRow.push(val);
          if (lastRow.length === maxColLen) {
            acc.push([]);
          }
          return acc;
        },
        [[]]
      );
    }
  },
  watch: {
    page(val) {
      const start = (val - 1) * PAGE_SIZE;
      const end = start + PAGE_SIZE;
      this.fetchRentals(start, end);
    }
  },
  methods: {
    async fetchRentals(start, end) {
      const resp = await api.get('rentals', {
        params: {
          start,
          end
        }
      });
      this.rentals = resp.data.rentals;
    },
    async fetchTotalRentals() {
      const resp = await api.get('rentals/total');
      this.total = Math.ceil(resp.data.total / PAGE_SIZE);
    }
  },
  async mounted() {
    this.fetchRentals(0, PAGE_SIZE);
    this.fetchTotalRentals();
  }
};
</script>

<style></style>
