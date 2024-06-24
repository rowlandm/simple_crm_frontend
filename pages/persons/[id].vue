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

      <!-- Note Form -->
      <h2>Add a Note</h2>
      <form @submit.prevent="submitForm">
        <div>
          <label for="note">Note</label>
          <textarea v-model="noteForm.note" id="note" required></textarea>
        </div>
        <div>
          <label for="date">Date</label>
          <input type="date" v-model="noteForm.date" id="date" required />
        </div>
        <button type="submit">Submit</button>
      </form>
    </div>

    <p><nuxt-link to="/">Go to Home Page</nuxt-link></p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRoute } from 'vue-router';
import { useAsyncData } from 'nuxt/app';

const { id } = useRoute().params;

const { data, error } = await useAsyncData('person', () =>
  $fetch('http://127.0.0.1:8000/persons/' + id)
);

if (error.value) {
  console.error('Error fetching person notes data:', error.value);
}

const noteForm = ref({
  person_id: id,
  note: '',
  date: ''
});

const submitForm = async () => {
  try {
    const response = await fetch('http://localhost:8000/notes/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(noteForm.value)
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    console.log('Response:', data);
    alert('Note submitted successfully!');

    // Optionally, refresh the notes data here if needed
    noteForm.value.note = '';
    noteForm.value.date = '';
  } catch (error) {
    console.error('Error submitting note:', error);
    alert('There was an error submitting the note.');
  }
};
</script>

<style scoped>
form {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
}
div {
  margin-bottom: 1em;
}
label {
  margin-bottom: 0.5em;
  font-weight: bold;
}
input,
textarea {
  padding: 0.5em;
  font-size: 1em;
}
button {
  padding: 0.5em;
  font-size: 1em;
  cursor: pointer;
}
</style>

