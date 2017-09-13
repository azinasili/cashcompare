<template>
<div id="app">
  <main>
    <input-group
      label="From"
      :total="convertFromValue"
      :country="convertFromCurrency"
      :countries="countries"
      v-on:value="updateConvertFromValue"
      v-on:country="updateConvertFromCurrency"></input-group>
    <input-group
      label="To"
      :total="convertRate"
      :country="convertToCurrency"
      :countries="countries"
      v-on:value="updateConvertToValue"
      v-on:country="updateConvertToCurrency"></input-group>
  </main>
</div>
</template>

<script>
import axios from 'axios';
import InputGroup from '@/containers/InputGroup';

export default {
  name: 'app',
  components: { InputGroup },
  data() {
    return {
      data: null,
      countries: ['USD', 'CAD', 'GBP', 'EUR'],
      convertToCurrency: '',
      convertFromCurrency: '',
      convertValue: null,
      convertToValue: null,
      convertFromValue: null,
    };
  },
  computed: {
    convertRate() {
      return parseInt(this.convertFromValue, 10) * this.convertValue;
    },
  },
  methods: {
    convertRequest() {
      if (this.convertFromCurrency && this.convertToCurrency) {
        axios.get(`http://api.fixer.io/latest?base=${this.convertFromCurrency}&symbols=${this.convertToCurrency}`)
          .then((response) => {
            this.data = response.data;
            this.convertValue = Object.values(this.data.rates)[0];
          })
          .catch(error => console.log(error));
      }
    },
    updateConvertToValue(value) {
      this.convertToValue = value;
    },
    updateConvertFromValue(value) {
      this.convertFromValue = value;
    },
    updateConvertToCurrency(value) {
      this.convertToCurrency = value;
      this.convertRequest();
    },
    updateConvertFromCurrency(value) {
      this.convertFromCurrency = value;
      this.convertRequest();
    },
  },
};
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
