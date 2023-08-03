<template>
  <div>
    <h1>Population Data</h1>
    <select v-model="selectedCountry" @change="fetchPopulationData">
      <option v-for="country in nation" :key="country.name" :value="country.name">
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
        nation: [],
      populationData: null,
    };
  },
  mounted() {
    this.fetchnation();
  },
  methods: {
    async fetchnation() {
      try {
        const response = await axios.get('https://datausa.io/api/data?drilldowns=Nation&measures=Population');
        this.nation = response.data;
      } catch (error) {
        console.error('Error fetching nation:', error);
      }
    },
    async fetchPopulationData() {
      try {
        const response = await axios.get(`https://datausa.io/api/data?drilldowns=Nation&measures=Population`);
        this.populationData = response.data[0];
      } catch (error) {
        console.error('Error fetching population data:', error);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
