<template>
    <div>
      <h1>Population Data</h1>
      <select v-model="selectedCountry" @change="fetchPopulationData">
        <option v-for="country in countries" :key="country.name" :value="country.name">
          {{ country.name }}
        </option>
      </select>
      <div v-if="populationData">
        <p>Country: {{ populationData.name }}</p>
        <p>Population: {{ populationData.population }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        selectedCountry: '',
        countries: [],
        populationData: null,
      };
    },
    mounted() {
      this.fetchCountries();
    },
    methods: {
      async fetchCountries() {
        try {
          const response = await axios.get('https://restcountries.com/v3/all');
          this.countries = response.data;
        } catch (error) {
          console.error('Error fetching countries:', error);
        }
      },
      async fetchPopulationData() {
        try {
          const response = await axios.get(`https://restcountries.com/v3/name/${this.selectedCountry}`);
          this.populationData = response.data[0];
        } catch (error) {
          console.error('Error fetching population data:', error);
        }
      },
    },
  };
  </script>