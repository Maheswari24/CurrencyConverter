<template>
  <div class="container">
    <h2>Currency List</h2>

    <input type="text" v-model="filterText" @input="filterData" placeholder="Filter Table"/>

    <table border="1" cellspacing="0" cellpadding="8">
      <thead>
        <tr>
          <th>Currency</th>
          <th>Online Sell Rate</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(value, key) in filteredCurrencies" :key="key">
          <td>{{ key }}</td>
          <td>{{ value }}</td>
        </tr>
        <tr v-if="filteredCurrencies.length === 0">
          <td colspan="4">No results found.</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { config } from "@/config.js";
import axios from "axios";

export default {
  name: "CurrencyTable",
  data() {
    return {
      config: {
        listapiEndpoint: config.API_URL_LIST,
        apiKey: config.API_KEY,
      },
      currencies: [],
      filteredCurrencies: [],
      filterText: "",
    };
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get(this.config.listapiEndpoint, {
          params: {
            api_key: this.config.apiKey,
          },
        });
        console.log(response.data.rates);
        this.currencies = response.data.rates;
        this.filteredCurrencies = this.currencies;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
    filterData() {
      const filterQuery = this.filterText.toLowerCase();

      this.filteredCurrencies = Object.entries(this.currencies).reduce(
        (acc, [key, value]) => {
          if (
            key.toLowerCase().includes(filterQuery) ||
            value.toString().includes(filterQuery)
          ) {
            acc[key] = value;
          }
          return acc;
        },
        {}
      );
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  width: 100%;
  padding: 20px; 
  box-sizing: border-box;
}
h2 {
  text-align: left;
}
input {
  margin-bottom: 10px;
  padding: 12px;
  border: 2px solid #888887;
  border-radius: 5px;
  line-height: 100%;
  font-size: 18px;
  display: flex;
  align-items: left;
  line-height: 1;
  width: 25%;
}
input:focus {
  background-color: #f3f1ed;  
  border-color: #8C8C8C;      
  outline: none;              
}
table {
  width: 100%;
  border: 0 none;
  border-collapse: collapse;
}
th {
  background-color: #f3f1ed;
}
th,
td {
  text-align: left;
  padding: 15px;
}
</style>
