<template>
<div id="input-group">
  <label for="convertFrom">{{label}}: {{country}}</label>
  <input type="number" id="convertFrom" v-model="total" v-on:change="onInputChange">
  <select v-model="country" v-on:change="onSelectChange">
    <option value="" disabled>Please select one</option>
    <option v-for="country in countries" :value="country">{{country}}</option>
  </select>
</div>
</template>

<script>
export default {
  name: 'input-group',
  props: ['label', 'total', 'country', 'countries'],
  methods: {
    onInputChange() {
      this.$emit('value', this.total);
    },
    onSelectChange() {
      this.$emit('country', this.country);
    },
  },
};
</script>

