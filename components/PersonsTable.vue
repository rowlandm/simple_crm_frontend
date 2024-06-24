<template>
  <div>
    <h1>Persons List</h1>
    <input v-model="searchQuery" placeholder="Search" />
    <table>
      <thead>
        <tr>
          <th @click="sortTable('full_name')">Full Name</th>
          <th @click="sortTable('email')">Email</th>
          <th @click="sortTable('job_title')">Job Title</th>
          <th @click="sortTable('id')">ID</th>
          <th @click="sortTable('last_note_date')">Last Note Date</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="person in filteredAndSortedPersons" :key="person.id">
          <td>{{ person.full_name }}</td>
          <td>{{ person.email }}</td>
          <td>{{ person.job_title }}</td>
          <td>{{ person.id }}</td>
          <td>{{ person.last_note_date }}</td>
          <td><nuxt-link :to="'persons/'+person.id">Go to Person Page</nuxt-link></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useAsyncData } from 'nuxt/app';

// Fetch data
const { data: persons, error } = await useAsyncData('persons', () => $fetch('http://127.0.0.1:8000/persons/'));

if (error.value) {
  console.error('Error fetching persons data:', error.value);
}

// State variables
const searchQuery = ref('');
const sortKey = ref('');
const sortAsc = ref(true);

// Computed property for filtered and sorted persons
const filteredAndSortedPersons = computed(() => {
  if (!persons.value) return [];

  let result = persons.value.filter(person => {
    return Object.values(person).some(value => String(value).toLowerCase().includes(searchQuery.value.toLowerCase()));
  });

  if (sortKey.value) {
    result.sort((a, b) => {
      let compareA = a[sortKey.value];
      let compareB = b[sortKey.value];
      
      if (sortKey.value === 'id') {
        compareA = Number(compareA);
        compareB = Number(compareB);
      }

      if (sortAsc.value) {
        return compareA > compareB ? 1 : compareA < compareB ? -1 : 0;
      } else {
        return compareA < compareB ? 1 : compareA > compareB ? -1 : 0;
      }
    });
  }

  return result;
});

// Method to sort table
const sortTable = (key) => {
  if (sortKey.value === key) {
    sortAsc.value = !sortAsc.value;
  } else {
    sortKey.value = key;
    sortAsc.value = true;
  }
};
</script>

<style>
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  background-color: #f4f4f4;
  cursor: pointer;
}
</style>

