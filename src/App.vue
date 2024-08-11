<script setup>
import { onMounted } from 'vue';
import HTable from './components/HTable.vue';
import axios from 'axios';
import { ref } from 'vue';
import Navigation from './components/Navigation.vue';
const columns = ['id', 'name', 'username', 'email'];
let sortedData = ref([]);
let sortOrder = ref(null);
let sortColumns = ref(null);
const locations = ref([
  'Lombard St, San Francisco, CA, USA',
  'PIER 39, The Embarcadero, San Francisco, CA, USA',
  'Golden Gate Bridge, San Francisco, CA, USA',
  `Fisherman's Wharf, San Francisco, CA, USA`,
  'Alcatraz Island, San Francisco, CA, USA'
]);

onMounted(() => {
  axios.get('https://jsonplaceholder.typicode.com/users')
    .then(response => sortedData.value = response.data);
});

function sortData(column) {
  if (sortedData.value === column) {
    sortOrder.value = sortOrder.value === 'asc' ? 'asc' : 'desc';
  } else {
    sortColumns.value = column;
    sortOrder = "asc";
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
