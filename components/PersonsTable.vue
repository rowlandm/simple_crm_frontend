<template>
  <div>
    <h1>Persons List</h1>
    <table>
      <thead>
        <tr>
          <th>Full Name</th>
          <th>Email</th>
          <th>Job Title</th>
          <th>ID</th>
          <th>Last Note Date</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="person in persons" :key="person.id">
          <td>{{ person.full_name }}</td>
          <td>{{ person.email }}</td>
          <td>{{ person.job_title }}</td>
          <td>{{ person.id }}</td>
          <td>{{ person.last_note_date }}</td>
    	  <td><nuxt-link :to="'persons/view/'+person.id">Go to Person Page</nuxt-link></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script setup>
import { useAsyncData } from 'nuxt/app';

const { data: persons, error } = await useAsyncData('persons', () => $fetch('http://127.0.0.1:8000/persons/'));

if (error.value) {
  console.error('Error fetching persons data:', error.value);
}
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
}
</style>

