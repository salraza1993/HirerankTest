<script setup>
import { onMounted } from 'vue';
import HTable from './components/HTable.vue';
import axios from 'axios';
import { ref } from 'vue';
import Navigation from './components/Navigation.vue';
const columns = ['id', 'name', 'username', 'email'];
let sortedData = ref([]);
let sortOrder = ref('asc');
let sortColumns = ref('');
const locations = ref([
  'Lombard St, San Francisco, CA, USA',
  'PIER 39, The Embarcadero, San Francisco, CA, USA',
  'Golden Gate Bridge, San Francisco, CA, USA',
  `Fisherman's Wharf, San Francisco, CA, USA`,
  'Alcatraz Island, San Francisco, CA, USA'
]);

onMounted(async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users');
    sortedData.value = response.data;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
});

function sortData(column) {
  if (sortColumns.value === column) {
    sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc';
  } else {
    sortColumns.value = column;
    sortOrder.value = "asc";
  }

  sortedData.value.sort((a, b) => {
    if (a[column] < b[column]) {
      return sortOrder.value === 'asc' ? -1 : 1;
    } else if (a[column] > b[column]) {
      return sortOrder.value === 'asc' ? 1 : -1;
    } else return 0;
  })
}

function moveUp(index) {
  if (index > 0) {
    const newLocations = [...locations.value];
    [newLocations[index], newLocations[index - 1]] = [newLocations[index - 1], newLocations[index]];
    locations.value = newLocations;
  }
}

function moveDown(index) {
  if (index < locations.value.length - 1) {
    const newLocations = [...locations.value];
    [newLocations[index], newLocations[index + 1]] = [newLocations[index + 1], newLocations[index]];
    locations.value = newLocations;
  }
}

</script>

<template>
  <div id="app">
    <div class="wrapper">
      <HTable :columns="columns" :sortedData="sortedData" @sort="sortData" />
      <br />
      <br />
      <Navigation :locations="locations" @moveUp="moveUp" @moveDown="moveDown" />
    </div>
  </div>
</template>

<style scoped>
</style>
