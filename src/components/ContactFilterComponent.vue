<template>
  <div class="filter-section">
    <label>Filter:</label>
    <input type="text" v-model="query" placeholder="Filter by Name and/or Email" @input="filter" />
  </div>
</template>

<script>
export default {
  name: 'ContactFilterComponent',
  data() {
    return {
      query: ''
    };
  },
  methods: {
    filter() {
      this.$emit('filter-contacts', this.query);
    }
  }
};
</script>

<style scoped>
.filter-section {
  margin-bottom: 30px;
}
label {
  margin-right: 10px;
}
input {
  padding: 5px;
}
</style>