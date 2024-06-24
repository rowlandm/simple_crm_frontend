<template>
  <div>
    <h1>Person Details</h1>
    <p>Person ID: {{ id }}</p>

	<div v-if="error">
      <p>Error loading data: {{ error.message }}</p>
    </div>
    <div v-else>
      <table>
        <tr>
          <th>Full Name</th>
          <td>{{ data.full_name }}</td>
        </tr>
        <tr>
          <th>Email</th>
          <td>{{ data.email }}</td>
        </tr>
        <tr>
          <th>Job Title</th>
          <td>{{ data.job_title }}</td>
        </tr>
        <tr>
          <th>ID</th>
          <td>{{ data.id }}</td>
        </tr>
      </table>

      <h2>Notes</h2>
      <table>
        <tr>
          <th>Person ID</th>
          <th>Note</th>
          <th>Date</th>
          <th>ID</th>
        </tr>
        <tr v-for="note in data.notes" :key="note.id">
          <td>{{ note.person_id }}</td>
          <td>{{ note.note }}</td>
          <td>{{ note.date }}</td>
          <td>{{ note.id }}</td>
        </tr>
      </table>
    </div>


    <p><nuxt-link to="/">Go to Home Page</nuxt-link> </p>
  </div>
</template>

<script setup>
    const { id } = useRoute().params

	import { useAsyncData } from 'nuxt/app';

	const { data,error } = await useAsyncData('person', () => $fetch('http://127.0.0.1:8000/persons/'+id));

	if (error.value) {
	  console.error('Error fetching person notes data:', error.value);
	}
</script>

<style scoped>
/* Add your styles here */
</style>

