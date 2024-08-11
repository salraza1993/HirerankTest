<script setup>
const props = defineProps({
  columns: Array,
  sortedData: Array
});
const emit = defineEmits(['sort']);
function onHeaderClick(column) {
  emit('sort', column);
}
</script>

<template>
  <div :style="{width: '100%'}">
    <table border="1">
      <thead>
        <tr>
          <th v-for="(head, headIndex) in columns" :key="headIndex" @click="onHeaderClick(head)">{{ head }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, itemIndex) in props.sortedData" :key="itemIndex">
          <td v-for="(column, columnIndex) in props.columns" :key="columnIndex">
            {{ item[column] }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<style scoped>
th.sorted {
  background-color: #4b737f;
}

table {
  border-right-color: #ffffff;
  font-size: 18px;
  border-collapse: collapse;
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
}

th,
td {
  padding: 10px 24px;
  text-align: left;
}

th {
  background-color: #087f5b;
  color: #fff;
  width: 25%;
  cursor: pointer;
}

tbody tr:nth-child(odd) {
  background-color: #f8f9fa;
}

tbody tr:nth-child(even) {
  background-color: #e9ecef;
}
</style>
