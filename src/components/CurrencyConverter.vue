<template>
  <div class="container">
    <h2>Currency Converter</h2>

    <div class="inputBox">
      <label for="country1">Amount:</label>
      <select id="country1" v-model="country1" @change="updateInput2">
        <option
          v-for="country in countries"
          :key="country.name"
          :value="country.name"
        >
          {{ country.name }}
        </option>
      </select>
      <input
        type="number"
        v-model.number="input1"
        @input="updateInput2"
        placeholder="Enter value for Country 1"
      />
    </div>

    <div class="inputBox">
      <label for="country2">Converted To:</label>
      <select id="country2" v-model="country2" @change="updateInput1">
        <option
          v-for="country in countries"
          :key="country.name"
          :value="country.name"
        >
          {{ country.name }}
        </option>
      </select>
      <input
        type="number"
        v-model.number="input2"
        @input="updateInput1"
        placeholder="Enter value for Country 2"
      />
    </div>
  </div>
   
</template>

<script>
import axios from "axios";
import { config } from "@/config.js";
import { debounce } from '@/utils/debounce';


export default {
  name: "CurrencyConverter",
  data() {
    return {
      config: {
        apiKey: config.API_KEY,
        currencyapiEndpoint: config.API_URL_CURRENCY,
        convertionapiEndpoint: config.API_URL_CONVERT,
      },
      countries: [], 
      country1: "INR", 
      country2: "GBP", 
      input1: 1000, 
      input2: "", 
    };
  },
  methods: {
    async fetchCountries() {
      try {
        const response = await axios.get(this.config.currencyapiEndpoint, {
          params: {
            api_key: this.config.apiKey,
          },
        });
        this.countries = response.data.response.map((country) => ({
          name: country.short_code,
        }));
      } catch (error) {
        console.error("Error fetching countries:", error);
      }
    },
    updateInput2: debounce(async function () {
      if (this.country1 && this.country2 && this.input1) {
        const response =  await this.fetchConversionRates(this.country1, this.country2, this.input1);
        if (response && response.data) {
            console.log(response.data.response.value)
        this.input2 = response.data.response.value.toFixed(2);
        }
      }
    },300),

    updateInput1: debounce(async function () {
      if (this.country1 && this.country2 && this.input2) {
        const response = await this.fetchConversionRates(
          this.country2,
          this.country1,
          this.input2
        );
        if (response && response.data) {
            console.log(response.data.response.value)
            this.input1 = response.data.response.value.toFixed(2);
        }
      }
    },300),
    async fetchConversionRates(from, to, amount) {
      try {
        const response = await axios.get(this.config.convertionapiEndpoint, {
          params: {
            api_key: this.config.apiKey,
            from,
            to,
            amount,
          },
        });
        return response;
      } catch (error) {
        console.error("Error Converting Values:", error);
      }
    },
  },
  mounted() {
    this.fetchCountries();
    this.updateInput2()
  },
};
</script>

<style scoped>
.container {
  padding: 10px; 
  margin: 20px auto; 
  box-sizing: border-box;
}

.inputBox {
    width: 50%;
    display: flex;
    flex-direction: row;
    margin: 20px auto;
}
input, select {
  margin-bottom: 10px;
  padding: 12px;
  border: 2px solid #888887;
  border-radius: 5px;
  line-height: 1;
  font-size: 18px;
  box-sizing: border-box;
}
input:focus, select:focus {
  background-color: #f3f1ed;  
  border-color: #8C8C8C;      
  outline: none;              
}
div {
  margin-bottom: 20px;
}
label {
  margin-right: 10px;
  display: flex;
  align-items: right;
  width: 25%;
  line-height: 45px;
  text-align: right;
}
select,
input {
  min-width: 50px;
}
select {
   border-radius: 4px 0 0 4px;
   border-right: 0;
}
input {
    border-radius: 0 4px 4px 0;
}
</style>
